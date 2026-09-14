# 02 — DNS versus transport failure

**Status:** Ready to run  
**Environment:** Windows PowerShell  
**Scenario:** A fictional user says a website is unavailable.

## Steps

1. Run `ipconfig /all`. Privately identify the active adapter, address, gateway and DNS resolver. Do not publish the full output.
2. Run these separately:
   ```powershell
   Resolve-DnsName example.com
   Resolve-DnsName no-such-host.invalid
   Test-NetConnection example.com -Port 443
   curl.exe -I --max-time 10 https://example.com
   ```
3. The .invalid name is a deliberate negative control. Its expected lookup failure does not mean your network is broken.
4. Record actual results. Network restrictions or a proxy can change the outcome; do not manufacture successful output.
5. Explain each layer: DNS resolves a name; TCP tests a connection; HTTP checks application-level response. A TCP success alone does not verify TLS or content.
6. Compare a browser result with curl. Record any difference in proxy behaviour or error message.
7. Write a closing note in French for a non-technical user, without claiming an outage was fixed.

## Decision guide

| Observation | Next check |
| --- | --- |
| Name lookup fails | Spelling, resolver reachability, relevant DNS record |
| Name resolves, TCP fails | Destination service, route, firewall policy |
| TCP works, HTTPS fails | Certificate, proxy, protocol and HTTP response |
| One browser fails | Browser-specific settings, extensions and session |

**Pass condition:** identify the intentional lookup failure and explain why changing DNS or disabling a firewall is not an evidence-based first action.

**Rollback:** none; no settings are changed.
