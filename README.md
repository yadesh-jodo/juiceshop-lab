# Juice Shop Isolated Lab

I built a local practice lab running OWASP Juice Shop in Docker on my Mac.

## How it is isolated

The container is bound to 127.0.0.1:3000, so only my own Mac can reach it.
I verified this by trying to open it from my phone on the same Wi-Fi,
and it failed to load (see the screenshots in this repo).

## How to reproduce

```bash
docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop
```

Then open http://127.0.0.1:3000 in a browser on the same Mac.

## Reset to clean baseline

```bash
docker rm -f juice-shop
docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop
```

## Files

- [Rules of Engagement](ROE.md)
- [Topology diagram](topology.png)
- Evidence screenshots: see the screenshot files in this repo
