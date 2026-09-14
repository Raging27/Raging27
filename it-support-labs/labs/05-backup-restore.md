# 05 — Verify a backup by restoring it

**Status:** Ready to run  
**Environment:** Docker; no real data or cloud account required.

## Create a synthetic dataset

Check container name `support-lab-backup` is unused. Run:
```sh
docker run --rm -it --name support-lab-backup alpine:3.22 sh
```

Inside the container:
```sh
mkdir -p /lab/source /lab/backup /lab/restored
printf 'ticket,status\nLAB-001,open\n' > /lab/source/tickets.csv
sha256sum /lab/source/tickets.csv
tar -czf /lab/backup/tickets.tar.gz -C /lab/source tickets.csv
tar -tzf /lab/backup/tickets.tar.gz
```

Record the original checksum. Listing the archive confirms membership, not that the backup is sufficient.

## Simulate corruption and recover elsewhere

```sh
printf 'corrupted test content\n' > /lab/source/tickets.csv
sha256sum /lab/source/tickets.csv
tar -xzf /lab/backup/tickets.tar.gz -C /lab/restored
sha256sum /lab/restored/tickets.csv
cat /lab/restored/tickets.csv
```

Compare the restored checksum to the original recorded checksum, not the corrupted source. Open the restored file and confirm the header and ticket row.

## Report

- What data did the backup contain?
- When was it taken?
- What would be lost if changes occurred after it?
- How long did your actual recovery take?
- Why is this same-container backup unsuitable as the only production backup?

**Pass condition:** matching original/restored checksums and readable content. Do not claim a database restore, disaster-recovery capability or production recovery time from this file-only exercise.

Save your evidence, then type `exit`. This removes the container including its synthetic data and archive.
