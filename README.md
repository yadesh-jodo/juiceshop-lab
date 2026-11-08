# Ethical Hacking Lab: Juice Shop

Practice repository for an isolated OWASP Juice Shop lab on my Mac.

| Task | What it covers | Link |
|---|---|---|
| Task 1 | Legal scope, isolated lab, rules of engagement | This page |
| Task 2 | Local recon and attack surface map | [recon/](recon/) |

---

# Task 1: Legal Scope and Isolated Lab

I built a local practice lab running OWASP Juice Shop in Docker on my Mac.

## How it is isolated

The container is bound to 127.0.0.1:3000, so only my own Mac can reach it.
I verified this by trying to open it from my phone on the same Wi-Fi,
and it failed to load (see the screenshots below).

## Topology

![Lab topology](topology.png)

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

## Evidence

**1. Juice Shop running on localhost**

![Juice Shop running](1-juice-shop-running.png)

**2. Phone on the same Wi-Fi cannot reach it**

![Phone blocked](2-phone-blocked.png)

**3. Docker port binding is 127.0.0.1 only**

![Docker port binding](3-docker-port.png)

## Files

- [Rules of Engagement](ROE.md)
- [Topology diagram](topology.png)

---

# Task 2: Recon and Attack Surface Map

I enumerated only the authorized lab target (127.0.0.1:3000) and turned the
results into a structured attack surface inventory with risk hypotheses.
Nothing was exploited.

- [Attack surface map](recon/ATTACK_SURFACE.md)
- [Task 2 summary and raw evidence](recon/)
