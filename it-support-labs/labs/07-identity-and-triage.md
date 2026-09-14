# 07 — Identity, MFA and incident prioritisation

**Status:** Ready to run  
**Type:** Tabletop exercise, no tenant or admin subscription required.

## Synthetic queue

| Ticket | Report | Evidence supplied |
| --- | --- | --- |
| A | One employee cannot sign in | Error says account locked; identity not yet verified |
| B | All staff cannot access booking app | Multiple independent reports beginning at 09:10 |
| C | A colleague needs a shared folder | No data-owner approval attached |
| D | An employee gets unexpected MFA prompts | They did not initiate sign-in |

## Task 1 — Triage

For each ticket write impact, urgency, first question, first safe check and escalation destination. Use provisional priorities and explain them; do not invent an employer SLA.

B warrants outage coordination. D warrants prompt security escalation. A needs identity verification before an approved unlock/reset workflow. C needs an access request and owner approval, not an administrator workaround.

## Task 2 — Identity runbook

Write the steps you would follow:

1. Confirm service, identifier, time and exact error.
2. Verify the requester using the approved identity process.
3. Determine whether the account is locked, disabled, expired or lacking entitlement.
4. Check authorised sign-in evidence.
5. Apply only the approved action. Do not ask for passwords or MFA codes.
6. Verify sign-in with the user and document the change.
7. For suspicious activity, follow the security team's containment process.

## Task 3 — Communications

Write a French acknowledgement for B containing: known impact, investigation status, a next-update time clearly marked as part of this simulation, and a workaround only if one has been verified.

Write an English escalation for D with facts, timestamps and actions already taken. Never claim compromise is confirmed from an unexpected prompt alone.

**Pass condition:** all four tickets have justified priorities and appropriate escalation; no account changes are claimed.

This exercise demonstrates written reasoning only. It is not evidence of hands-on Active Directory, Entra ID or Microsoft 365 administration.
