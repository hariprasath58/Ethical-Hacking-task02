\# Authorized Lab Asset Inventory

**Owner:** [HARIPRASATH B]
**Scope reference:** see `01-authorization.md`
**Rule:** Only the systems marked "In scope" may be tested.

## 1. Asset table

| ID | Asset | Owner | Address / location | Software & version | Data class | Identities | Scope | Evidence |
|---|---|---|---|---|---|---|---|---|
| A1 | Juice Shop web application | [HARIPRASATH B] | 127.0.0.1:3000 | OWASP Juice Shop (bkimminich/juice-shop:latest), Node.js | Public (training shop content) | Anonymous visitor, seeded customer accounts, seeded admin account | **In scope** | `02-juice-shop-localhost.png` |
| A2 | Juice Shop REST API | [HARIPRASATH B] | 127.0.0.1:3000 (paths under /rest and /api) | Part of A1 | Internal (fake user and order data) | Same as A1 | **In scope** | `02-juice-shop-localhost.png` |
| A3 | Juice Shop database | [HARIPRASATH B] | Inside container (file-based, no network port) | SQLite (bundled with Juice Shop) | Sensitive (fake credentials, fake personal data) | App service account | **In scope** (only through A1/A2) | n/a |
| A4 | Docker container `keen_neumann` | [HARIPRASATH B] | Local, port mapping 127.0.0.1:3000 to 3000/tcp | Image bkimminich/juice-shop:latest (334 MB) | Internal | Container root user | **In scope** | `01-docker-running.png`, `04-versions.png` |
| A5 | Docker Engine | [HARIPRASATH B] | Kali VM | Docker 28.5.2+dfsg4 | Internal | root / docker group | Out of scope (platform only) | `04-versions.png` |
| A6 | Kali Linux VM | [HARIPRASATH B] | Local virtual machine | Kali Linux, kernel 6.19.14+kali-amd64, x86_64 | Internal | User `kali` | Out of scope (testing workstation) | `05-kernel-info.png` |
| A7 | Physical host computer and home/campus network | [HARIPRASATH B] / network provider | Outside the VM | n/a | **Sensitive (real personal data)** | Personal accounts | **OUT OF SCOPE, never test** | n/a |

## 2. Network exposure check

The lab listens on 127.0.0.1:3000 only, so it is not reachable from other devices (evidence: `03-localhost-binding.png`).

## 3. Trust boundaries

| ID | Boundary | Between |
|---|---|---|
| TB1 | Browser to web app | Untrusted user input enters Juice Shop |
| TB2 | App to database | Application logic to stored data |
| TB3 | Container to Kali VM | Docker isolation layer |
| TB4 | Kali VM to physical host / network | Lab to real world (must stay closed) |

## 4. Data classes used

- **Public:** content anyone may see (product catalog)
- **Internal:** lab configuration and logs
- **Sensitive:** credentials and personal data (fake in the lab), plus any real data on the host

## 5. Likely attack surfaces (in scope only)

| Asset | Attack surface |
|---|---|
| A1 | Login form, registration form, search box, feedback form, product reviews, file upload, basket and checkout, admin page |
| A2 | Authentication endpoints, user and order endpoints, parameters and tokens |
| A3 | Any input that reaches database queries (via A1/A2) |
| A4 | Exposed port mapping, container configuration |
