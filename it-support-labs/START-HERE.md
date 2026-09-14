# Start here

This portfolio contains guided, simulated support exercises. All labs start as **Ready to run**. It is a learning project, not a record of employment or resolved customer incidents.

## Environment

- Windows labs: your own Windows machine or disposable Windows VM, Windows PowerShell, no administrator rights needed for baseline checks.
- Container labs: a machine with Docker already installed and running. Open Terminal on macOS/Linux. On Windows, use PowerShell and substitute curl.exe for curl.
- Linux permissions lab: Docker provides the isolated Linux environment; no Linux installation is required.
- Use only synthetic data. No production accounts, employer systems or paid cloud deployment is needed.
- Run each command individually. Read the output before continuing. If a command fails, record it rather than skipping silently.
- Commands use specific lab container names. If a name already exists, inspect it and stop; do not remove an unfamiliar container.

## Before starting a container lab

Run `docker version`. Both client and server should respond. If the server is unavailable, open your Docker application and retry. Confirm you understand any applicable licence and local hardware requirements before installing software.

Run `docker ps -a`. Check that the lab's container name is unused.

Image tags may change over time. Record the actual image ID, OS and tool versions with your results. These instructions have been reviewed as text but have not been executed in this session.

## Completion standard

For each lab, save a copy of [the ticket template](templates/ticket.md) with your actual observations. Include:

1. Before state and one reproducible symptom.
2. At least two checks and what they establish.
3. A root cause supported by evidence.
4. The fix and its scope.
5. A successful retest of the original task.
6. One limitation or unresolved question.

Screenshots are optional; sanitised text evidence is often clearer. Never publish tokens, passwords, database URLs, personal emails, browser cookies or customer logs.

## Suggested execution order

1. Windows baseline or DNS (30–45 minutes).
2. Docker outage (45 minutes).
3. Linux permissions (30 minutes).
4. Backup recovery (45 minutes).
5. SQL investigation (45 minutes).
6. Identity and triage tabletop (30 minutes).

Estimates are study budgets, not completion claims. Start applying while completing labs. Add only the skills you can explain and reproduce to your CV.
