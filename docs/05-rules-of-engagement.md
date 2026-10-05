# Rules of Engagement

**Tester:** HARIPRASATH B
**Authorizer:** HARIPRASATH B
**Related documents:** `01-authorization.md`, `02-asset-inventory.md`, `04-stride-register.md`

## 1. Scope reminder
Only OWASP Juice Shop at 127.0.0.1:3000 (Docker container `keen_neumann`) may be tested. Everything else is out of scope.

## 2. Test windows
- **Allowed days and hours:** Mon-Sat, 6:00 PM to 9:00 PM IST
- **Authorized period:** 05 Oct 2026 to 05 Nov 2026
- **Outside these windows:** no testing. The container may stay running but must remain bound to 127.0.0.1.
- **Before each session:** confirm the lab is local-only with `sudo ss -tlnp | grep 3000` (must show 127.0.0.1).

## 3. Exclusions (never touch)
- Physical host computer and anything outside the Kali VM
- Home or campus router, Wi-Fi network and other devices on it
- Any internet-facing website, server or service
- Internet service provider infrastructure
- Cloud, email and social media accounts
- Any account or data belonging to another person
- Denial-of-service testing that could affect the host or network (testing is limited to the container)

## 4. Allowed testing activities
- Manual and tool-assisted testing of Juice Shop's web pages and its own REST API, using only seeded or self-created lab accounts
- Reviewing the threats in `04-stride-register.md`
- Snapshots or restarts of the container to restore a clean state

## 5. Evidence handling
| Item | Rule |
|---|---|
| Storage | Keep evidence in a local folder inside the Kali VM, then publish only reviewed copies to the repository |
| Naming | Number files in order with a short description (example: `01-docker-running.png`) |
| Redaction | Before publishing, remove or blur real names, passwords, tokens, personal emails and anything from the real host |
| Lab data | Fake lab accounts and data may be shown. Real data may never be published |
| Integrity | Do not edit or crop screenshots in a way that changes their meaning. Keep original files unchanged |
| Retention | Keep evidence until the course task is reviewed, then delete local copies not needed |
| Sharing | Share only through the public repository for this task |

## 6. Escalation
| Situation | Action | Contact |
|---|---|---|
| Unclear whether something is in scope | Stop and ask before continuing | RabTech Academy course instructor |
| Lab behaves unexpectedly (crash, high CPU, strange traffic) | Stop testing, record what happened, restore the container | Tester, then instructor |
| Real personal data found | Stop, do not copy or publish it, report it | Instructor |
| Possible real vulnerability found in third-party software | Do not exploit further. Record details and report responsibly | Instructor |
| Mistake in scope or process | Document it in the log and tell the instructor | Instructor |

**Contacts:** Tester: HARIPRASATH B. Instructor: RabTech Academy course instructor (via the Student Learning Desk).

## 7. Stop conditions (stop immediately if any occur)
1. Any traffic from the lab or test tools leaves the Kali VM toward the physical host, local network or internet
2. `ss -tlnp` shows the lab listening on `0.0.0.0` instead of `127.0.0.1`
3. The container, Docker or the Kali VM crashes or becomes unresponsive
4. Real personal data, real credentials or a real system is discovered
5. The target is no longer clearly the authorized Juice Shop container
6. The authorized period ends or the test window closes
7. The tester is unsure of scope, authorization or the next action

**After stopping:** record what happened with the time, run `sudo docker stop keen_neumann`, review, and resume only after the cause is fixed and scope is confirmed.

## 8. Reporting
Findings are written up with: ID, affected component, evidence file name, impact, and recommended fix, using the IDs from `04-stride-register.md`.

## 9. Acknowledgement
I will follow these rules for the whole authorized period.

**Signed:** HARIPRASATH B  **Date:** 05 Oct 2026
