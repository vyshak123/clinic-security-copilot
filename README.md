# Clinic Security Copilot

A secure Azure environment for a fictional Sydney GP clinic: strong identity controls, patient records kept off the public internet, and automated detection of suspicious sign-in and data-access activity.

> **Status:** 🚧 In progress — Phase 0 (setup) complete. All patient data in this project is synthetic.

## 1. Business problem

Small Australian healthcare clinics hold highly sensitive patient data (diagnoses, medications, Medicare details) but rarely have dedicated security staff. Credential theft is a leading cause of health-sector breaches — the 2022 Medibank breach began with stolen login credentials.

This project simulates a GP clinic's cloud environment that:

- **Stops stolen-password attacks** with MFA and location-based Conditional Access
- **Removes permanent admin access** with just-in-time Privileged Identity Management (PIM)
- **Keeps patient records off the internet** using a private endpoint and Managed Identity (no secrets in code)
- **Detects suspicious behaviour within minutes** using KQL detection rules in Log Analytics
- **Explains alerts in plain English** for non-technical staff using an AI triage function

Controls are aligned to the ACSC Essential Eight strategies for **multi-factor authentication** and **restricting administrative privileges**.

## 2. Architecture

See [docs/architecture.md](docs/architecture.md).

## 3. Build phases

| Phase | Layer | Azure / tools | Status |
|---|---|---|---|
| 0 | Repo, tools, design | GitHub, draw.io | ✅ Done |
| 1 | Account + cost guardrails | Cost Management, Azure Policy | ⏳ |
| 2 | Clinic app (container) + CI/CD | Docker, Azure Container Apps, GitHub Actions | ⏳ |
| 3 | Identity | Entra ID: MFA, Conditional Access, PIM | ⏳ |
| 4 | Workload lockdown | VNet, NSG, Private Endpoint, Managed Identity | ⏳ |
| 5 | Monitoring + detections | Log Analytics, KQL, Alert rules | ⏳ |
| 6 | Infrastructure as code | Terraform, GitHub Actions (OIDC, approval gates) | ⏳ |
| 7 | Security dashboard | Power BI | ⏳ |
| 8 | AI alert triage | Azure Functions + LLM | ⏳ |

## 4. Key decisions

_To be completed as the build progresses. Planned:_

| Decision | Chosen | Instead of | Why |
|---|---|---|---|
| App hosting | Azure Container Apps | AKS | Cost and simplicity at clinic scale |
| Network protection | NSGs + Private Endpoint | Azure Firewall | Cost; sufficient for this scope |
| App → storage auth | Managed Identity | Storage keys | No secrets to leak |
| Admin access | PIM (just-in-time) | Permanent admin roles | Least privilege |
| Pipeline auth | OIDC federated credentials | Stored client secrets | No long-lived credentials |

## 5. Detections

_Coming in Phase 5._

## 6. Cost management

- Budget alert: $5/month (50% / 80% / 100%)
- Azure Policy: Australia East only, small VM sizes only
- All resources tagged `project=clinic-lab`
- Final cost: _TBD_

## 7. Troubleshooting log

See [docs/troubleshooting.md](docs/troubleshooting.md).

## 8. What I would change for production

Azure Firewall, Azure Front Door with WAF, Key Vault, Microsoft Sentinel, Defender for Cloud, multi-region resilience.

## Repository structure

```
app/            Clinic patient-portal app (FastAPI + Docker)
infra/          Terraform
detections/     KQL detection rules
dashboard/      Power BI report and screenshots
ai-triage/      Azure Function for AI alert triage
docs/           Architecture, troubleshooting log, screenshots
```
