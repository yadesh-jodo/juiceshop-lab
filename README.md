# Juice Shop Isolated Lab

I built a local practice lab running OWASP Juice Shop in Docker on my Mac.

## How it is isolated
The container is bound to 127.0.0.1:3000, so only my own Mac can reach it.
I verified this by trying to open it from my phone on the same Wi-Fi,
and it failed to load (see docs/evidence).

## How to reproduce
docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop

## Reset to clean baseline
docker rm -f juice-shop
docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop

## Files
- [Rules of Engagement](ROE.md)
- [Topology diagram](docs/topology.png)
- Evidence screenshots: docs/evidence/