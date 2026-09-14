# 04 — File permissions without broad access grants

**Status:** Ready to run  
**Environment:** Docker; all writes are inside a disposable container.

## Scenario

A test user should be able to read a document but cannot. Fix access for its intended owner without making it readable by everyone.

1. Confirm container name `support-lab-permissions` is unused.
2. Open an interactive Linux shell:
   ```sh
   docker run --rm -it --name support-lab-permissions alpine:3.22 sh
   ```
3. Inside that shell, run:
   ```sh
   adduser -D analyst
   mkdir /lab
   printf 'Synthetic support document\n' > /lab/report.txt
   chmod 600 /lab/report.txt
   ls -l /lab/report.txt
   su analyst -c 'cat /lab/report.txt'
   ```
4. Record the failure. The file is owned by root and readable/writable only by its owner.
5. In this simulation the approved data owner is analyst. Apply the narrow correction:
   ```sh
   chown analyst:analyst /lab/report.txt
   su analyst -c 'cat /lab/report.txt'
   ls -l /lab/report.txt
   ```
6. Add another synthetic user and check that access remains restricted:
   ```sh
   adduser -D outsider
   su outsider -c 'cat /lab/report.txt'
   ```
7. Explain owner/group/other permissions and why chmod 777 would be excessive.
8. Rollback demonstration:
   ```sh
   chown root:root /lab/report.txt
   su analyst -c 'cat /lab/report.txt'
   ```
9. Type `exit`. Docker removes this disposable container and its contents. Save sanitised evidence first.

**Pass condition:** analyst can read after the approved ownership change, outsider cannot, and rollback restores the original restriction.
