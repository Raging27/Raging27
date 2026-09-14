# IT Support Lab Portfolio

**Windows · networking · Linux · Docker · recovery · SQL · incident communication**

A practical learning portfolio for junior IT support and application-support roles, connecting workstation troubleshooting with web-application operations.

**7 guided exercises are ready to run. Execution evidence is pending.** Scenarios are simulated and prepared with AI assistance. Completed results will be recorded only after actual execution.

## Start here

[Environment and completion guide](START-HERE.md) · [Ticket template](templates/ticket.md) · [Interview checklist](INTERVIEW-CHECKLIST.md)

## Lab catalogue

| Lab | Skill | Deliverable | Status |
| --- | --- | --- | --- |
| [01 — Windows baseline](labs/01-windows-diagnostics.md) | Processes, memory, disks, event evidence | Workstation diagnostic report | Ready to run |
| [02 — DNS and networking](labs/02-dns-network.md) | DNS vs TCP vs HTTP | Layer-by-layer investigation | Ready to run |
| [03 — Docker outage](labs/03-docker-outage.md) | Logs, service state, recovery | Before/failure/after evidence | Ready to run |
| [04 — Linux permissions](labs/04-linux-permissions.md) | Ownership and least privilege | Access test and rollback | Ready to run |
| [05 — Backup recovery](labs/05-backup-restore.md) | Restore validation and checksums | Verified file recovery | Ready to run |
| [06 — SQL investigation](labs/06-sql-investigation.md) | Read-only queries and escalation | Duplicate-booking investigation | Ready to run |
| [07 — Identity and triage](labs/07-identity-and-triage.md) | MFA, access requests, priorities | French/English ticket communications | Ready to run |

## What this portfolio shows

The guides define repeatable troubleshooting methods, bounded changes, verification and escalation. They do not yet demonstrate completed practical work, production experience or Microsoft tenant administration.

No live credentials or production data are required. Docker exercises run locally and do not deploy anything to Railway.

## Recommended first session

Complete lab 03 if Docker is already available, or lab 01 on Windows. Publish one sanitised ticket with actual results before moving to the next exercise.

## Additional reference runbooks

The original four scenario outlines are retained below for reference.


Practical preparation for junior IT support and application-support interviews.

**Status: exercise specifications and runbooks, not completed incidents.** Scenarios below are fictional. There are no claimed customers, resolutions, timings or successful test results. Written with AI assistance; execution and evidence are pending.

## Working method

1. Confirm the symptom, affected users, business impact and last known working state.
2. Collect evidence before making changes.
3. Test one hypothesis at a time, starting with the least disruptive check.
4. Obtain approval for changes and record a rollback plan.
5. Verify the original workflow, document the result and escalate when needed.

Use only your own disposable lab machines and synthetic accounts. Never publish credentials, tokens, personal data, employer logs or unredacted screenshots.

## Lab 1 — Website unavailable: DNS, network or application?

**Fictional ticket:** One user cannot open a website. Other sites work.

**Environment:** Windows lab machine with PowerShell. Choose a hostname you are authorised to test.

### Investigation

1. Record the exact URL, browser error and time. Check whether another browser and another device show the same symptom.
2. Run `ipconfig /all`. Identify the active adapter, address, default gateway and configured DNS server. Do not post the raw output publicly.
3. Run `Resolve-DnsName example.com` as a control, then repeat using the affected hostname.
4. Run `Test-NetConnection example.com -Port 443`, substituting the affected hostname when testing it.
5. Separate the findings:
   - DNS lookup fails: investigate the resolver, name spelling and DNS records.
   - DNS succeeds but TCP fails: investigate reachability, service availability and approved network policy.
   - TCP succeeds but the browser fails: inspect the actual HTTP/TLS/browser error.
6. Record whether a proxy or VPN is involved. Do not disable managed protection or bypass organisational DNS policy.

**Important:** a successful TCP test does not prove HTTPS works. A failed ping does not prove the destination is down.

**Controlled exercise:** compare a normal lookup with `Resolve-DnsName no-such-host.invalid`. The second is an intentional negative control, not evidence of a real outage.

**Acceptance:** explain which layer failed, support it with sanitised output, and retest the original URL after an approved fix. Do not change DNS or flush caches without a reason.

## Lab 2 — Application unreachable on localhost

**Fictional ticket:** A local development application does not load.

**Environment:** Your own local app; no production changes.

### Investigation

1. Record the URL and expected port from the app configuration.
2. Check whether the application process is running and whether startup reported an error.
3. On Windows use `Test-NetConnection localhost -Port 3000` if 3000 is the configured port. On macOS/Linux use `curl -I http://localhost:3000`.
4. Compare the requested port with the listening port. Connection refusal is different from HTTP 404 or 500.
5. For a 500 response, inspect the matching local application log entry. Redact secrets and user data.
6. Check required services and configuration without copying secrets into the ticket.
7. Apply only a fix supported by the evidence; restart only the affected local process if needed.

**Controlled exercise:** start your disposable local app, record its normal response, stop it normally, compare the connection error, then start it again.

**Acceptance:** capture before/after responses and explain the distinction between a stopped service, wrong port and application exception.

## Lab 3 — User cannot sign in

**Fictional ticket:** A user reports that their usual account no longer works.

### Investigation

1. Confirm the affected service, exact error, last successful sign-in and whether other users are affected.
2. Distinguish incorrect username, expired password, locked account, missing entitlement, MFA problem and service outage.
3. Verify identity using the organisation's approved process before any account change.
4. Use authorised sign-in logs to correlate the timestamp and error. Record only sanitised evidence.
5. Do not request the user's password, MFA code or recovery codes.
6. If a reset is authorised, use the approved reset workflow. Never disable MFA as a shortcut.
7. Escalate suspicious sign-ins or unsolicited MFA prompts through the security process.

**Controlled exercise:** use a disposable test account in an app you control. Compare a valid sign-in with one intentional incorrect-password attempt. Do not trigger a lockout by repeatedly guessing.

**Acceptance:** document the observed message, evidence, correct escalation path and successful user workflow if an authorised fix was performed.

## Lab 4 — File access denied

**Fictional ticket:** A user can open a shared folder but cannot edit one document.

### Investigation

1. Determine whether the failure affects one file, one folder or all shared resources.
2. Check whether the file is locked by another process, read-only or subject to access restrictions.
3. Confirm the intended access with the data owner.
4. Compare the user's effective access and group membership with the approved policy.
5. Remember that remote access can depend on both share permissions and filesystem permissions.
6. Make only the approved minimum access change. Do not grant broad access or local administrator rights to work around the problem.
7. Verify permitted operations and confirm that unrelated access has not expanded.

**Controlled exercise:** on a disposable local folder, use two test accounts with intentionally different permissions. Record which account can read and write. Do not change system folders or real shared-drive permissions.

**Acceptance:** explain the cause and the least-privilege correction, including an explicit rollback plan.

## Evidence template

Copy this section for each exercise actually completed:

- Exercise:
- Date and lab environment:
- Status: not started / in progress / completed
- Synthetic symptom:
- Expected behaviour:
- Actual observations:
- Hypotheses:
- Checks performed and sanitised results:
- Root cause supported by evidence:
- Change made and authorisation:
- Rollback:
- Verification of original workflow:
- User-facing closing message:
- What remains unresolved:
- What I learned:

Do not mark an exercise completed until the steps and verification have actually been performed.
