# Rules of Engagement

## Authorization
I own and control every system in scope. Testing any system I do not
own or have written permission to test is prohibited.

## Target (in scope)
- OWASP Juice Shop in Docker on my Mac at http://127.0.0.1:3000 only

## Allowed techniques
- Manual web testing, browser dev tools, Burp Suite against 127.0.0.1:3000
- Exploiting Juice Shop's intentional vulnerabilities (XSS, SQLi, broken auth)
- Password attacks only on accounts I created in the lab

## Exclusions (never allowed)
- Any address other than 127.0.0.1:3000
- My Mac's system, home router, other devices, cloud services, third-party sites
- Denial-of-service or anything that exhausts my machine
- Changing the container to bind to 0.0.0.0 or exposing it to the network

## Stop conditions
Stop immediately if:
- Any traffic goes to an out-of-scope address
- The container becomes reachable from another device
- A tool misbehaves (runaway scan, Mac freezing)
- I am unsure whether something is in scope