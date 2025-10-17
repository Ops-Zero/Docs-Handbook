# 🧭 DevOps Zero — Engineering Governance Document

**Version:** 1.0  
**Owner:** Platform Team  
**Last Updated:** {{date}}  
**Applies To:** All repositories and teams within the `devops-zero` GitHub organization.

---

## 🏢 1. Organization Overview

**DevOps Zero** is a zero-cost, open-source DevOps learning and operations platform designed to simulate real-world infrastructure, automation, and delivery environments.

The purpose of this governance is to ensure:

* Every change is reviewed, traceable, and reversible.  
* Every repo follows consistent security, release, and review standards.  
* Every failure produces a measurable lesson (runbook, alert, or improvement).

---

## ⚙️ 2. Organizational Structure

| Team | Focus Area | Primary Repos | Role |
| ---- | ----------- | -------------- | ---- |
| **Platform** | CI/CD, automation, GitHub Actions | `platform-ci` | Admin & enforcement |
| **Infra** | Terraform, Ansible, networking | `infra-terraform`, `infra-ansible` | Infra provisioning |
| **Apps** | Python & Go microservices | `apps-python-api`, `apps-go-service` | Application development |
| **K8s** | Kubernetes cluster management | `k8s-platform` | Deployment & scaling |
| **Monitoring** | Observability, metrics, dashboards | `monitoring-stack` | SRE/monitoring |
| **Docs** | Policies, standards, onboarding | `docs-handbook` | Documentation authority |

Each team is assigned explicit access (see **Access Control Policy**).

---

## 🔒 3. Access Control Policy

**Principle:** *Least Privilege + Separation of Duties*

| Repo | Platform | Infra | Apps | K8s | Monitoring | Docs | Default (All Members) |
|------|-----------|--------|------|------|-------------|-----------|
| platform-ci | **Admin** | Read | Read | Read | Read | Read | Read |
| infra-terraform | Maintain | **Write** | Read | Read | Read | Read | Read |
| infra-ansible | Maintain | **Write** | Read | Read | Read | Read | Read |
| apps-python-api | Maintain | Write | **Write** | Read | Read | Read | Read |
| apps-go-service | Maintain | Write | **Write** | Read | Read | Read | Read |
| k8s-platform | Maintain | Write | Write | **Maintain** | Read | Read | Read |
| monitoring-stack | Maintain | Write | Write | Write | **Write** | Read | Read |
| labs-troubleshooting | Maintain | **Write** | Read | Read | **Write** | Read | Read |
| docs-handbook | Maintain | Write | Write | Write | Write | **Write** | Read |

**Org-Wide Security Defaults:**

* Base permissions: `Read`  
* 2FA: **Required for all members**  
* Repository creation: **Owners only**  
* Branch deletion: **Disabled for non-admins**  
* Actions workflow permissions: default `read-all`  
* Forking allowed for public repos  

---

## 🛡️ 4. Security & Compliance Policy

**Objective:** Ensure all repositories maintain security integrity, review discipline, and observability.

### Authentication & Authorization

* 2FA enforced for all members and collaborators.  
* Access granted only through teams (no direct user permissions).

### Branch Protection

* `main` branch protected across all repos.  
* Required:  
  * Pull request for all changes  
  * Minimum 1 approving reviewer  
  * All status checks pass before merge  
  * No direct commits to protected branches  
  * Dismiss stale reviews on new commits

### Vulnerability Management

* Dependabot alerts enabled on all repos.  
* Secret scanning and CodeQL security analysis enabled (where available).  
* High/Critical vulnerabilities must be remediated within 48 hours.

### Audit & Monitoring

* Org audit log reviewed weekly (manual on free tier).  
* Access review every sprint.  
* Incidents documented using runbooks (see section 7).

---

## 🔄 5. Development Lifecycle & Workflow

**Objective:** Maintain predictable and auditable software delivery.

### Branching Strategy

| Branch | Purpose | Protection |
| ------- | -------- | --------- |
| `main` | Stable, production-ready | Protected |
| `feature/*` | Development work | Merged via PR |
| `hotfix/*` | Urgent fixes | Reviewed, tagged, merged |

### Code Review & Merge Policy

* Every PR must be linked to a Jira issue (DZ-###).  
* Self-approvals are prohibited.  
* CI workflows (`lint`, `build`, `test`) must pass before merging.  
* Commits must be signed or verified.

### Release Policy

* Semantic Versioning (`vX.Y.Z`)  
* Releases created automatically via CI after passing tests.  
* Build artifacts (containers) pushed to GHCR (`ghcr.io/devops-zero/...`).

### Continuous Integration

* Reusable workflows defined in `platform-ci` repo.  
* All repos must call standard CI templates via `workflow_call`.  
* No repo-specific workflows unless approved by Platform.

---

## ✅ 6. Definitions

### Definition of Ready (DoR)

A ticket is **Ready** when:

* It has a clear title and problem statement.  
* Acceptance criteria are defined.  
* Story points are estimated.  
* Dependencies are identified.  
* Reviewed and accepted by Platform lead.

### Definition of Done (DoD)

A task is **Done** when:

* Code merged to `main`.  
* CI pipelines pass.  
* Documentation updated.  
* Alerts and dashboards (if applicable) configured.  
* No open critical vulnerabilities.  
* Jira issue moved to “Done” and demoed in sprint review.

---

## 🚀 7. Change Control & Release Management

### Change Policy

* All changes must occur via Pull Request.  
* Emergency changes (hotfix) require post-mortem documentation.  
* No direct merges to `main`.

### Release Cadence

* Standard release: per sprint or as needed after QA.  
* Hotfix releases: on-demand with incident record.  
* All releases are automatically tagged and built via GitHub Actions.

### Rollback Policy

* Each release must have a rollback tag or backup deployment.  
* Rollback procedures documented in relevant runbook.

---

## 📉 8. Incident Response & Runbooks

**Objective:** Build operational maturity through documented incident handling.

* All incidents must have a post-mortem in `/docs-handbook/docs/runbooks/`.  
* Each runbook includes:
  * **Symptoms**
  * **Root Cause**
  * **Resolution Steps**
  * **Prevention Measures**
* Mean Time to Resolution (MTTR) tracked per incident.  
* Major incidents trigger retro discussion within next sprint.

---

## 🧩 9. Observability & Monitoring Standards

* Every deployed service must expose Prometheus metrics (`/metrics`).  
* Dashboards defined in Grafana for:
  * Availability (uptime %)
  * Latency (p95, p99)
  * Error rates (4xx, 5xx)
  * Resource usage (CPU, Memory)
* Alerts must be configured before marking any service “Production-ready.”

---

## 🧠 10. Continuous Improvement & Governance Evolution

* Governance reviewed once per quarter.  
* All modifications to this file require a PR reviewed by Platform lead.  
* Retrospective findings feed into updated policies.

---

## 🪪 11. Document Ownership

| Section | Owner | Review Cadence |
| -------- | ------ | -------------- |
| Access Control | Platform | Quarterly |
| Security Policy | Platform + Infra | Bi-weekly |
| Development Workflow | Apps + Platform | Each sprint |
| Incident Process | Monitoring | Post-incident |
| Documentation | Docs Team | Continuous |

---

## 🏁 12. Governance Document Definition of Done (Meta)

This document is **considered complete** when:

1. It covers all operational domains (Access, Security, Delivery, Monitoring).  
2. It is stored in `docs-handbook` under version control.  
3. It is approved by the Platform owner (you).  
4. It is referenced by all engineering teams and repositories.

---

## 🧾 Change History

| Version | Date | Author | Change |
| -------- | ---- | ------- | ------- |
| 1.0 | {{date}} | Platform Team | Initial governance publication |
