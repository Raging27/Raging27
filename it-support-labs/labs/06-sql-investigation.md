# 06 — Investigate duplicate bookings with SQL

**Status:** Ready to run  
**Environment:** Docker and PostgreSQL container; no host port published.

## Start isolated PostgreSQL

Check `support-lab-db` is unused:
```sh
docker run -d --name support-lab-db -e POSTGRES_PASSWORD=lab-only-password postgres:17-alpine
docker exec support-lab-db pg_isready -U postgres
```

The password is a public disposable-lab value, never a production credential. Wait until pg_isready reports readiness; initialization may take time.

Open psql:
```sh
docker exec -it support-lab-db psql -U postgres
```

Create synthetic records inside psql:
```sql
CREATE DATABASE support_lab;
\c support_lab
CREATE TABLE bookings (
  id integer PRIMARY KEY,
  user_id integer NOT NULL,
  trip_id integer NOT NULL,
  status text NOT NULL
);
INSERT INTO bookings VALUES
(1, 101, 501, 'confirmed'),
(2, 101, 501, 'confirmed'),
(3, 102, 501, 'cancelled'),
(4, 103, 502, 'confirmed');
```

## Investigate read-only

```sql
BEGIN READ ONLY;
SELECT user_id, trip_id, COUNT(*) AS confirmed_count
FROM bookings
WHERE status = 'confirmed'
GROUP BY user_id, trip_id
HAVING COUNT(*) > 1;

SELECT id, user_id, trip_id, status
FROM bookings
WHERE user_id = 101 AND trip_id = 501
ORDER BY id;
ROLLBACK;
```

Expected fixture result: user 101 has two confirmed rows for trip 501. This is expected output to compare against your actual run.

## Explain the boundary

Duplicate rows are evidence of a data condition, not proof of the application cause. Possible causes include a retry, missing validation or a missing uniqueness constraint. Request application logs and clarify whether repeat bookings are allowed before proposing a correction.

Do not delete a row merely because it looks duplicated; financial and downstream records may depend on it.

**Pass condition:** actual query results plus an escalation note distinguishing the observed duplication from an unproven cause.

Exit using `\q`. After saving evidence:
```sh
docker stop support-lab-db
docker rm -v support-lab-db
```
The final command removes only this lab container and its anonymous data volume.
