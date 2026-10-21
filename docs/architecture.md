# Architecture

Every path to patient records passes identity checks and is logged.

```mermaid
flowchart LR
    staff["Clinic staff<br/>doctors, nurses, reception, IT"]
    entra["Microsoft Entra ID<br/>MFA · Conditional Access · PIM"]

    subgraph vnet["VNet: vnet-clinic"]
        subgraph snetapp["snet-app"]
            app["Container App<br/>patient portal<br/>Managed Identity"]
        end
        subgraph snetdata["snet-data"]
            pe["Private Endpoint"]
            st[("Storage<br/>patient-records<br/>public access OFF")]
        end
    end

    law["Log Analytics<br/>KQL detections"]
    alerts["Alert rules<br/>email"]
    ai["AI triage<br/>Azure Function + LLM"]
    issue["GitHub issue<br/>triage ticket"]
    pbi["Power BI<br/>security dashboard"]
    gha["GitHub Actions + Terraform<br/>OIDC · manual approval"]

    staff -->|sign in| entra
    entra -->|authorised access| app
    app -->|reads via identity| pe --> st
    entra -->|sign-in & audit logs| law
    app -->|app logs| law
    st -->|storage read logs| law
    law --> alerts --> ai --> issue
    law --> pbi
    gha -->|deploys| vnet
```

## Components

| Component | Purpose |
|---|---|
| Entra ID | Identity for all staff; MFA, Conditional Access (Australia only), PIM for just-in-time admin |
| Container App | The clinic's patient portal; self-heals via health probe |
| Private Endpoint + Storage | Patient records reachable only from inside the VNet |
| Managed Identity | App reads storage with no keys or passwords |
| NSGs | Data subnet accepts traffic only from the app subnet |
| Log Analytics + KQL | Central logs and anomaly detection rules |
| AI triage | Turns alerts into plain-English tickets |
| Power BI | Trends and anomalies for the practice manager |
| Terraform + GitHub Actions | Repeatable, reviewed, approved deployments |

_A draw.io version will be added as `docs/architecture.png`._
