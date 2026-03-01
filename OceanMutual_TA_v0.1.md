# OceanMutual — Technical Assignment (TA v1.0)

## 0) Scope Summary (MVP)
- **Single founder node** (federation later).
- **Public read-only**, **registered-anonymous reporting** allowed (anti-abuse + audit trail).
- **Core MVP features (A, B, C):**
  - **A. Forums with Critical Topics → Proposals → Voting**
  - **B. Action Plans (Project Boards) auto-created on approval**
  - **C. Whistleblowing/Reporting module** (secure forms, routing, tracker)
- **Off-chain voting** with signed ballots + public proofs; migration path to **on-chain attestations** later.
- **Curated API integrations** for paid/critical tools (no marketplace yet).
- **Embedded, open-source chat** (Matrix rooms) for **project participants only**.
- **AGPL-3.0** license; contributions must remain open.

## 1) Goals & Non-Goals
### Goals
1. Deliver a working, **member-verified professional network** with public read-only access.
2. Ship **topic forums** that enforce the pipeline: *Discussion → Proposal → Vote → Action Plan (if approved)*.
3. Ship **whistleblowing/reporting** with secure routing and a redacted public tracker.
4. Provide **role-gated project management** (Kanban + milestones + private chat).
5. Publish **transparent, queryable activity metrics** (dashboard v0).

### Non-Goals (MVP)
- Federation across multiple nodes.
- Treasury and on-chain budgets.
- Full service marketplace (we do **curated API** only).
- DMs, personal feeds, or non-professional content.

## 2) Users, Roles & Permissions
- **Visitor (Public):** read-only topics, proposals (non-sensitive), redacted case tracker, dashboard.
- **Registered Member (pseudonymous allowed):** can create proposals, vote, comment, file reports; must accept ToS & Code of Conduct.
- **Verified Seafarer (post-MVP toggle):** required for **voting** once verification is live; in MVP, voting allowed for registered members (flag this in UI).
- **Moderator:** topic moderation, proposal gating, report triage, redaction.
- **Project Lead/Contributor:** access to private project rooms, task boards, status updates.
- **Board/Admin:** system config, role assignment, approvals, escalation routing.

**Anonymous but registered** reporting: member must be logged in (account-level audit), but report can be **publicly anonymous** (PII auto-redacted; full details visible only to moderators/board).

## 3) Information Architecture
### Entities
- **CriticalTopic** → has **Goals**, **Resources**, **Proposals** (by status), **Archives**
- **Proposal** → lifecycle: *Draft → Discussion (timeboxed) → Finalized → Vote → Approved/Rejected → (if approved) ActionPlan*
- **ActionPlan (Project)** → Kanban, milestones, metrics, Matrix room
- **CaseReport** → encrypted intake, routing, status events, redacted public view
- **Vote** → signed ballot, tally, IPFS proof
- **Event** → system activity for dashboard and webhooks

### Key Views
- Topics index / Topic detail  
- Proposal detail (discussion thread, attachments, timeline)  
- Voting page (with quorum/threshold info + signed ballot)  
- Project board (Kanban + milestones + private chat)  
- Reporting intake → report detail (role-gated) → public tracker (redacted)  
- Dashboard (counts, trends, recent events)

## 4) System Architecture (MVP)
**Single node** with modular services:

```
[ Next.js Web App ]  -->  [ API Gateway (Node/Express or Fastify) ] --> [ Services ]
                                                      |-> Auth Service (OIDC/OAuth2)
                                                      |-> Topic/Proposal Service
                                                      |-> Voting Service (Ballot signer, tally, proofs)
                                                      |-> Project Service (Kanban, milestones)
                                                      |-> Matrix Bridge (project rooms)
                                                      |-> Reporting Service (encrypted intake, routing)
                                                      |-> Search Service (Meili/OpenSearch)
                                                      |-> Metrics/Events Service (dashboard)
                                                      |-> File Service (S3-compatible + IPFS pinning)
DB: PostgreSQL  |  Queue: Redis  |  Object: S3/MinIO  |  Immutable: IPFS (Pinata/self-hosted) 
Observability: OpenTelemetry + Prometheus + Grafana + Loki
```

**Tech choices (recommended)**  
- **Frontend:** Next.js (React 18), TypeScript, Tailwind, i18n (next-intl).  
- **API:** Node.js (Fastify) + TypeScript; OpenAPI spec.  
- **DB:** PostgreSQL + Prisma ORM.  
- **Search:** MeiliSearch → OpenSearch later.  
- **Queue:** Redis (BullMQ).  
- **Chat:** Matrix Synapse + Element Web embedded.  
- **Immutable artifacts:** IPFS for proposal/vote snapshots.  
- **Auth:** Keycloak/Authentik; JWT access tokens.  
- **CI/CD:** GitHub Actions; Docker; Helm charts later.  
- **License:** **AGPL-3.0** (root).

## 5) Data Model (initial schema)
- `users`, `profiles`, `topics`, `proposals`, `votes`, `projects`, `tasks`, `reports`, `events`, `api_keys`.

## 6) Core Workflows
### Proposal Workflow
Draft → Discussion (timeboxed) → Finalize & Vote → Result → Project or Archive.

### Voting
Signed ballots, duplicate prevention, final tally with IPFS proof.

### Action Plans
Auto-created project boards; private Matrix chat; audit logs.

### Reporting
Encrypted forms, routing, redacted public tracker, reporter view.

## 7) Security & Privacy
RBAC, encryption, audit logs, anti-abuse, verified emails, 2FA for moderators.

## 8) Internationalization & Accessibility
i18n, WCAG 2.1 AA compliance.

## 9) Curated API Integrations
Curated partners only; community value pledge; limited scopes.

## 10) DevOps & Repo
License: AGPL-3.0; GitHub Actions; Docker; Helm; observability stack.

## 11) Testing & Acceptance
Unit, integration, and security tests; manual QA; load test.

## 12) Rollout
Alpha → Beta → Public MVP.

## 13) Roadmap
MVP (8–10 wks) → M1 (4–6 wks) → M2 (6–8 wks).

## 14) Backlog Epics
Topics & Proposals, Voting, Action Plans, Reporting, Dashboard.

## 15) Definition of Done
MVP flows tested, one live proposal approved → project active, reporting works, docs complete, license verified.
