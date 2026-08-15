# Spoural Games — Threat Model

## 1. Purpose

The purpose of this document is to identify the assets,
trust boundaries, threat actors, security threats, potential
impact, prevention controls, detection methods, and recovery
considerations for the Spoural Games platform.

---

## 2. Protected Assets

| Asset | Importance |
|---|---|
| Admin accounts | Critical |
| Student accounts | High |
| Auction bids | Critical |
| Team budgets | Critical |
| Auction results | Critical |
| Match results | High |
| Player information | High |
| Database | Critical |
| Audit logs | High |
| Backups | Critical |
| Public tournament information | Low |

---

## 3. Threat Actors

- Anonymous internet attacker
- Malicious or compromised student account
- Compromised team manager
- Compromised administrator
- Attacker exploiting vulnerable dependencies
- Automated bots or abusive clients

---

## 4. Trust Boundaries

### Boundary 1
Browser → Frontend

### Boundary 2
Frontend → Express API

### Boundary 3
API → PostgreSQL

### Boundary 4
Browser → Socket.IO

### Boundary 5
Administrator → Administrative operations

---

## 5. Security Principles

- Least privilege
- Defense in depth
- Secure by default
- Server-side authorization
- Data minimization
- Input validation
- Fail safely
- Audit sensitive operations
- Protect confidentiality, integrity, and availability

---

## 6. Threat Register

| ID | Threat | Asset | Impact | Likelihood | Main Controls |
|---|---|---|---|---|---|
| T001 | Account compromise | User/Admin accounts | High | Medium | Password hashing, rate limiting, secure sessions, MFA for admins |
| T002 | Broken access control | Admin functions | Critical | Medium | Server-side RBAC and authorization |
| T003 | SQL injection | Database | Critical | Medium | Parameterized queries, validation, least-privilege DB account |
| T004 | XSS | User/browser data | High | Medium | Safe rendering, output encoding, CSP, validation |
| T005 | Unauthorized bid | Auction data | Critical | Medium | Server-side validation, authorization |
| T006 | Invalid team budget | Team budget | Critical | Medium | Server-side calculation and database constraints |
| T007 | Auction race condition | Auction state | Critical | Medium | Transactions and concurrency control |
| T008 | Unauthorized result modification | Match results | High | Medium | RBAC, approval workflow, audit logging |
| T009 | Sensitive data exposure | Student data | High | Medium | Data minimization, authorization, safe API responses |
| T010 | Denial of service | API | High | Medium | Rate limiting, request limits, monitoring |
| T011 | Malicious file upload | Server/storage | High | Low/Medium | Type/size/content validation, safe storage |
| T012 | Dependency vulnerability | Application | High | Medium | Dependency review, lockfile, vulnerability scanning |
| T013 | Secret exposure | Credentials | Critical | Medium | Environment variables, secret management, secret scanning |
| T014 | WebSocket abuse | Live auction | Critical | Medium | Socket authentication and authorization |
| T015 | Database compromise | All data | Critical | Low/Medium | Network isolation, least privilege, strong credentials, backups |

---

## 7. Detection

Potential detection mechanisms include:

- Security logs
- Audit logs
- Failed login monitoring
- Authorization failure monitoring
- Rate-limit events
- Database error monitoring
- Dependency vulnerability alerts
- Infrastructure monitoring

---

## 8. Response

When a security incident is suspected:

1. Detect
2. Contain
3. Investigate
4. Recover
5. Fix the root cause
6. Review and improve controls
7. Notify appropriate university personnel when required

---

## 9. Recovery

Recovery mechanisms include:

- Tested database backups
- Recovery procedures
- Audit trails
- Secure configuration backups
- Incident documentation

---

## 10. Review Policy

This threat model must be reviewed whenever:

- A major feature is added
- Authentication changes
- Auction logic changes
- Database architecture changes
- File uploads are introduced
- New external services are added
- The application moves to production
- A security incident occurss