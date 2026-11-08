# Attack Surface Map: OWASP Juice Shop (Local Lab)

## 1. Target and scope
- Target: http://127.0.0.1:3000
- Isolation: Docker container bound to 127.0.0.1 only, see Task 1
- Scope check: only 127.0.0.1:3000 was touched, matches ROE.md
- Date: 2026-11-08

## 2. Services and versions
| Port | State | Service | Version | Source |
|---|---|---|---|---|
| 3000/tcp | open | HTTP web app | Juice Shop 20.3.0 | nmap -sV, /rest/admin/application-version |

## 3. HTTP headers observed
| Header | Value | Note |
|---|---|---|
| Access-Control-Allow-Origin | * | Any website can read responses |
| X-Content-Type-Options | nosniff | Present |
| X-Frame-Options | SAMEORIGIN | Present |
| Feature-Policy | payment 'self' | Deprecated header |
| X-Recruiting | /#/jobs | Custom header leaking a route |
| Content-Security-Policy | missing | No script restrictions |
| Strict-Transport-Security | missing | No HTTPS enforcement |

Methods allowed (nmap OPTIONS): GET, HEAD, PUT, PATCH, POST, DELETE.

## 4. Routes found
| Route | How found | Notes |
|---|---|---|
| / | curl | Single-page app |
| /robots.txt | curl | Disallow: /ftp |
| /ftp/ | robots.txt hint | Public directory listing (see 4a) |
| /rest/admin/application-version | curl | Returns version without login |
| /#/jobs | X-Recruiting header | Hidden route |

## 4a. Exposed /ftp/ contents
Listing only. No files were downloaded or opened.

| File | Type | Risk hypothesis |
|---|---|---|
| acquisitions.md | Business document | Confidential info exposed |
| announcement_encrypted.md | Encrypted document | Weak encryption possible |
| coupons_2013.md.bak | Backup | Old coupon codes, abuse |
| eastere.gg | Unusual extension | Hidden file |
| encrypt.pyc | Compiled Python | Logic could be reverse-engineered |
| incident-support.kdbx | KeePass database | Password vault exposed |
| legal.md | Business document | Internal legal text exposed |
| package.json.bak | Backup | Reveals dependency versions |
| package-lock.json.bak | Backup | Exact versions help match CVEs |
| quarantine/ | Folder | Possibly sensitive files |
| suspicious_errors.yml | Config or log | May leak internals |

## 5. Visible inputs
| Page | Input | Notes |
|---|---|---|
| Home | Search box | Query reflected in page |
| Login | Email, password | Authentication |
| Register | Email, password, security question | Account creation |
| Contact | Comment, rating, captcha | Feedback form |

## 6. Observations and risk hypotheses
| # | Observation | Evidence | Risk hypothesis |
|---|---|---|---|
| 1 | CORS allows all origins | headers.txt | Other sites could read API responses |
| 2 | No Content-Security-Policy | headers.txt | XSS impact is higher |
| 3 | No HSTS | headers.txt | Traffic could be downgraded |
| 4 | robots.txt lists /ftp | robots.txt | Points attackers to sensitive paths |
| 5 | /ftp/ public with .bak, .kdbx, .pyc files | ftp-files.txt | Backup, credential and source files exposed |
| 6 | Unauthenticated version endpoint | version.txt | Helps match known vulnerabilities |
| 7 | X-Recruiting header | headers.txt | Leaks hidden route |
| 8 | PUT, PATCH, DELETE allowed | nmap.txt | Possible broken access control |
| 9 | Search box reflects input | Home page | Possible reflected XSS |
| 10 | Login form | Login page | Possible SQL injection or weak auth |

These are hypotheses only. Nothing was exploited.

## 7. Priority for next testing
| Priority | Target | Why |
|---|---|---|
| High | /ftp/ exposure | 11 sensitive-looking files, easiest to verify |
| High | Login form | Auth bypass gives wide access |
| High | REST API with PUT/PATCH/DELETE | Access control flaws |
| Medium | Search box | Reflected input, no CSP |
| Medium | CORS wildcard | Depends on authenticated endpoints |
| Low | Missing HSTS, Feature-Policy | Low direct impact |
