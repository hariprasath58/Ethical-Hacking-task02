# STRIDE Threat Register

**Scope:** OWASP Juice Shop at 127.0.0.1:3000 (see `01-authorization.md`, `02-asset-inventory.md`)
**Method:** STRIDE applied to each element and data flow in `03-data-flow-diagram.md`
**Scoring:** Likelihood (1-5) x Impact (1-5) = Score. High: 15+, Medium: 8-14, Low: 7 or less.

## 1. Threat register (ranked)

| Rank | ID | STRIDE | Component | Threat | Abuse case | L | I | Score | Band | Mitigation |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | T01 | Tampering | Login, search (flow 1, 3) | Untrusted input changes database queries (injection) | Attacker crafts input in a form field to read or change data they should not reach | 5 | 5 | 25 | High | Parameterized queries, input validation, least-privilege database account |
| 2 | T02 | Spoofing | Login | Weak or guessable credentials on seeded accounts | Attacker guesses a password to log in as admin and gain full control | 4 | 5 | 20 | High | Strong password policy, account lockout and rate limiting, MFA |
| 3 | T03 | Elevation of privilege | REST API, basket and user endpoints | Broken access control | Logged-in customer changes an identifier in a request to view or modify another user's data | 4 | 5 | 20 | High | Server-side authorization check on every request, ownership validation |
| 4 | T04 | Information disclosure | REST API responses | Excessive data returned to client | Attacker reads API responses that include fields the page never shows (emails, hashes) | 4 | 4 | 16 | High | Return only needed fields, response filtering, never return password hashes |
| 5 | T05 | Tampering | Feedback, reviews, search display | Stored or reflected script injection (XSS) | Attacker saves script in a review so it runs in other users' browsers and steals sessions | 4 | 4 | 16 | High | Output encoding, input sanitization, Content-Security-Policy |
| 6 | T06 | Spoofing | Session token | Token forgery or theft | Attacker obtains or forges a token to act as another user without a password | 3 | 5 | 15 | High | Strong signing keys, short token lifetime, HttpOnly and Secure cookies |
| 7 | T07 | Elevation of privilege | Registration | Role set by client | Attacker adds an extra role parameter during signup to register as admin | 3 | 5 | 15 | High | Ignore client-supplied roles, assign roles server-side only |
| 8 | T08 | Tampering | File upload | Unsafe file handling | Attacker uploads an unexpected file type or oversized file to alter or overload the server | 3 | 4 | 12 | Medium | File type and size allow-list, store outside web root, rename files |
| 9 | T09 | Repudiation | Application logging | Missing audit trail | User or attacker performs an admin action and later denies it because nothing was logged | 4 | 3 | 12 | Medium | Log authentication, admin and order events with user ID and time, protect log integrity |
| 10 | T10 | Information disclosure | Lab network exposure | Lab reachable beyond localhost | Container started on 0.0.0.0 exposes a vulnerable app to other devices on the network | 2 | 5 | 10 | Medium | Bind to 127.0.0.1 only, verify with `ss -tlnp` (evidence 03), firewall rule |
| 11 | T11 | Information disclosure | Error handling | Verbose error messages | Attacker triggers an error and learns software paths, versions or query details | 3 | 3 | 9 | Medium | Generic error pages for users, details only in server logs |
| 12 | T12 | Denial of service | Web app | Resource exhaustion | Attacker sends many rapid requests so legitimate users cannot use the shop | 3 | 3 | 9 | Medium | Rate limiting, request size limits, resource limits on the container |
| 13 | T13 | Repudiation | Feedback and orders | Actions attributable to the wrong user | A user denies submitting feedback because identity was not verified server-side | 3 | 3 | 9 | Medium | Bind submissions to the authenticated user ID, keep records |
| 14 | T14 | Denial of service | Docker container | Crash from malformed input | Unexpected input crashes the Node.js process and the shop goes offline | 2 | 3 | 6 | Low | Input validation, restart policy, health checks |
| 15 | T15 | Elevation of privilege | Container to Kali VM | Container escape | Attacker breaks out of the container and gains access to the VM | 1 | 5 | 5 | Low | Keep Docker updated, run as non-root, no privileged flag, no extra mounts |

## 2. STRIDE coverage check

| Category | Threat IDs |
|---|---|
| Spoofing | T02, T06 |
| Tampering | T01, T05, T08 |
| Repudiation | T09, T13 |
| Information disclosure | T04, T10, T11 |
| Denial of service | T12, T14 |
| Elevation of privilege | T03, T07, T15 |

## 3. Top mitigations by priority

1. **Validate input and use parameterized queries** (T01, T05, T08, T14)
2. **Enforce server-side authorization on every request** (T03, T07)
3. **Strengthen authentication and sessions** (T02, T06)
4. **Limit data in responses and errors** (T04, T11)
5. **Add logging, rate limiting and container hardening** (T09, T12, T15)

## 4. Notes
- This register is a planning artifact. No testing has been performed yet.
- All testing stays within the scope in `01-authorization.md` and follows `05-rules-of-engagement.md`.
