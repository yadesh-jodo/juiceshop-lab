# Task 2: Local Reconnaissance and Attack Surface Map

I enumerated only the authorized lab target (`http://127.0.0.1:3000`, OWASP Juice Shop in Docker, bound to localhost) and converted the observations into a structured attack surface inventory with risk hypotheses. Nothing was exploited.

## Scope check

- Target: 127.0.0.1:3000 only
- Isolation: Docker container bound to localhost (see the Task 1 README in the repo root)
- Rules: all activity stayed inside [ROE.md](../ROE.md)

## Main deliverable

- [ATTACK_SURFACE.md](ATTACK_SURFACE.md): services, headers, routes, /ftp contents, visible inputs, risk hypotheses and testing priorities

## Commands used

```bash
docker port juice-shop
nmap -sV -p 3000 127.0.0.1 -oN nmap.txt
curl -sI http://127.0.0.1:3000 > headers.txt
curl -s http://127.0.0.1:3000/robots.txt > robots.txt
curl -s http://127.0.0.1:3000/rest/admin/application-version > version.txt
curl -s http://127.0.0.1:3000/ftp/ | grep -o 'href="[^"]*"' | sort -u > ftp-files.txt
```

## Raw evidence files

| File | What it contains |
|---|---|
| target.txt | Target address and port binding |
| nmap.txt | Service and version scan of port 3000 |
| headers.txt | HTTP response headers |
| robots.txt | robots.txt content |
| version.txt | Application version endpoint |
| ftp-listing.txt | Raw /ftp/ response (truncated) |
| ftp-files.txt | File names found in /ftp/ |

## Key findings

- Only one open port: 3000/tcp (Juice Shop 20.3.0)
- CORS allows all origins, no CSP, no HSTS
- robots.txt points to /ftp, and the directory listing is public with 11 files and 1 folder (backups, a KeePass database, compiled Python)
- Version endpoint is available without login
- PUT, PATCH and DELETE methods are allowed

## Next to test (priority)

1. /ftp/ exposure
2. Login form
3. REST API access control
