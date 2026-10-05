# Task 02: Legal Scope, Asset Inventory & Threat Model

**Course:** Cybersecurity & Ethical Hacking, RabTech Academy
**Author:** [HARIPRASATH B]

## Summary
Authorized lab dossier for a local, intentionally vulnerable OWASP Juice Shop running in Docker on a Kali Linux VM. The lab is bound to `127.0.0.1:3000`, so it is not reachable from any other device. No real system was tested.

## Deliverables

| # | Document | What it contains |
|---|---|---|
| 1 | [Written authorization](docs/01-authorization.md) | Owner, tester, dates, in-scope target, out-of-scope list |
| 2 | [Asset inventory](docs/02-asset-inventory.md) | Assets, owners, identities, data classes, trust boundaries, attack surfaces |
| 3 | [Data-flow diagram](docs/03-data-flow-diagram.md) | Flows between browser, web app and database with trust boundaries |
| 4 | [STRIDE register](docs/04-stride-register.md) | 15 ranked threats with abuse cases and mitigations |
| 5 | [Rules of engagement](docs/05-rules-of-engagement.md) | Test windows, exclusions, evidence handling, escalation, stop conditions |

## Evidence
| File | Shows |
|---|---|
| [01-docker-running.png](evidence/01-docker-running.png) | Container running, port mapped to 127.0.0.1 |
| [02-juice-shop-localhost.png](evidence/02-juice-shop-localhost.png) | Juice Shop open at 127.0.0.1:3000 |
| [03-localhost-binding.png](evidence/03-localhost-binding.png) | Port 3000 listening on localhost only |
| [04-versions.png](evidence/04-versions.png) | Docker version and image |
| [05-kernel-info.png](evidence/05-kernel-info.png) | Kali Linux kernel and architecture |

## Lab setup
```
sudo docker run -d -p 127.0.0.1:3000:3000 bkimminich/juice-shop
```
