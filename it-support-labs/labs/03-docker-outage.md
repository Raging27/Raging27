# 03 — Diagnose and recover a stopped web service

**Status:** Ready to run  
**Environment:** Docker and a terminal  
**Goal:** Produce a reproducible before/failure/after incident record.

## Create a disposable service

Check the name `support-lab-web` is unused with `docker ps -a`. Run:

```sh
docker run -d --name support-lab-web -p 127.0.0.1:18080:80 nginx:alpine
docker ps --filter name=support-lab-web
curl -I --max-time 5 http://127.0.0.1:18080
docker logs --tail 20 support-lab-web
```

On Windows use `curl.exe`. If port 18080 is occupied, stop and choose another unused port consistently. The service is bound to localhost.

Record the image used:
```sh
docker inspect --format '{{.Image}}' support-lab-web
```

## Introduce one fault

```sh
docker stop support-lab-web
curl -I --max-time 5 http://127.0.0.1:18080
docker ps -a --filter name=support-lab-web
docker inspect --format '{{.State.Status}} {{.State.ExitCode}}' support-lab-web
```

Record the actual client error. The deliberate stop is the known cause in this exercise. In a real incident, an exited container alone would not explain why it stopped.

## Recover and verify

```sh
docker start support-lab-web
curl -I --max-time 5 http://127.0.0.1:18080
docker logs --tail 20 support-lab-web
```

Also open the URL in a browser. Compare the before/after response and verify the content, not just process existence.

## Retain or clean up

Stop the container when finished. To delete only this disposable service after saving your evidence:
```sh
docker stop support-lab-web
docker rm support-lab-web
```

**Pass condition:** show normal response, failure, stopped-container evidence and successful recovery. Explain the difference between restoring service and establishing an unknown root cause.

Reference: [Docker container run](https://docs.docker.com/reference/cli/docker/container/run/).
