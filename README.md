
# Cloud Security & Operations: Azure vs AWS vs GCP

This report maps each selected **Azure** service to its closest **AWS** and **GCP** equivalents and compares them across:
- **Overview**
- **Core Features**
- **Security & Compliance**
- **Pricing Model**
- **Integration for DevSecOps**

---

## 🔎 Quick Reference Mapping

# Azure, AWS, and GCP Service Equivalents

This document maps 18 Azure services from the Week 13 Revision slides to their closest AWS and GCP equivalents.

| #  | Azure Service | AWS Equivalent | GCP Equivalent |
|----|---------------|----------------|----------------|
| 1  | **Azure Resource Manager (ARM)** | AWS CloudFormation / AWS CDK | Google Cloud Deployment Manager (legacy) / Config Connector / Terraform |
| 2  | **Microsoft Entra ID (Azure AD)** | AWS IAM + IAM Identity Center (formerly AWS SSO) | Google Cloud Identity + Cloud IAM |
| 3  | **Azure Monitor** | Amazon CloudWatch | Google Cloud Monitoring (Ops Suite) |
| 4  | **Azure Log Analytics** | CloudWatch Logs + Logs Insights | Google Cloud Logging (+ BigQuery for analysis) |
| 5  | **Azure Policy** | AWS Config + AWS Organizations SCPs | Google Cloud Organization Policy Service + Policy Controller |
| 6  | **Azure Landing Zone** | AWS Control Tower | Google Cloud Landing Zone Framework (Blueprints) |
| 7  | **Microsoft Defender for Cloud** | AWS Security Hub + Amazon Inspector + GuardDuty | Google Security Command Center |
| 8  | **Microsoft Sentinel** | AWS Security Hub + Amazon Detective | Chronicle Security Operations (Google) |
| 9  | **Azure Logic Apps** | AWS Step Functions + AWS EventBridge + AWS Lambda (for workflows) | Google Cloud Workflows + Eventarc |
| 10 | **Azure Functions** | AWS Lambda | Google Cloud Functions |
| 11 | **Azure Event Hubs** | Amazon Kinesis Data Streams / Amazon MSK (Kafka) | Google Cloud Pub/Sub |
| 12 | **Azure Storage** | Amazon S3 | Google Cloud Storage |
| 13 | **Azure Key Vault** | AWS Secrets Manager + AWS KMS | Google Secret Manager + Cloud KMS |
| 14 | **Azure Firewall** | AWS Network Firewall | Google Cloud Firewall |
| 15 | **Azure Network Security Groups (NSG)** | AWS Security Groups | Google VPC Firewall Rules |
| 16 | **Azure Blueprints** | AWS Service Catalog + AWS Control Tower | Google Cloud Deployment Manager + Terraform + Org Policy Bundles |
| 17 | **Azure DevOps** | AWS CodePipeline + CodeBuild + CodeCommit + CodeDeploy | Google Cloud Build + Artifact Registry + Cloud Deploy |
| 18 | **Azure API Management** | Amazon API Gateway | Google Cloud Endpoints / API Gateway |

---

## 1) Azure Resource Manager (ARM) vs CloudFormation vs Deployment Manager/Config Connector

### Overview
- **Azure ARM**: Azure’s control plane and IaC engine for declarative deployments (via ARM JSON or Bicep). Manages dependencies, idempotency, and RBAC scopes.
- **AWS CloudFormation**: Declarative IaC for AWS resources (YAML/JSON). CDK adds higher-level languages. Integrates with StackSets and Service Catalog.
- **GCP Deployment Manager / Config Connector**: Deployment Manager is legacy declarative IaC; **Config Connector** manages GCP resources via Kubernetes CRDs; Terraform is widely used for GCP.

### Core Features
- **ARM**: Bicep/JSON templates, template specs, what-if, deployment stacks, scoped deployments (tenant/subscription/RG), RBAC integration.
- **CloudFormation**: Change sets, drift detection, StackSets (multi-account/region), modules, CDK constructs, Service Catalog portfolios.
- **GCP**: Deployment Manager (templates), **Config Connector** (GitOps via K8s), **KRM** policy integration, strong Terraform ecosystem.

### Security & Compliance (high level)
- All three providers operate under common programs such as **ISO 27001**, **SOC 1/2/3**, **PCI DSS** (service-dependent), **FedRAMP** (regions/programs vary). IaC engines inherit platform compliance; resource compliance depends on the target services.

### Pricing Model
- **ARM**: No direct cost for the engine; you pay for provisioned resources.
- **CloudFormation**: No additional charge (except for StackSets with external deployments in some scenarios); pay for resources.
- **GCP**: Deployment Manager/Config Connector have no direct charge; pay for resources. (Kubernetes costs apply for Config Connector clusters.)

### Integration for DevSecOps
- **ARM**: Azure DevOps/GitHub Actions, template specs, policy-as-code with **Azure Policy**, previews (what-if), RBAC.
- **CloudFormation**: CI/CD with AWS CodePipeline/GitHub; policy guardrails via **SCPs/Config/Proton**; CDK testing.
- **GCP**: GitOps via **Config Connector** and **Config Sync**; CI/CD with Cloud Build/GitHub; org policies and **Policy Controller** for guardrails.

---

## 2) Microsoft Entra ID vs AWS IAM/Identity Center vs Google Cloud Identity/IAM

### Overview
- **Entra ID (Azure AD)**: Cloud identity provider for users, apps, and devices with **SSO**, **MFA**, **Conditional Access**, and B2B/B2C scenarios.
- **AWS IAM + IAM Identity Center**: IAM manages principals/permissions; **Identity Center** provides SSO to AWS accounts/apps and ties to external IdPs.
- **Google Cloud Identity + IAM**: Cloud Identity handles identities/SSO/MFA; **Cloud IAM** manages permissions on GCP resources.

### Core Features
- **Entra ID**: SSO (SAML/OIDC), Conditional Access, MFA, PIM (Privileged Identity Management), B2B/B2C, device compliance, app gallery, Enterprise Apps.
- **AWS**: Fine-grained IAM policies, roles, STS, Identity Center SSO to accounts/apps, permission sets, federation via SAML/OIDC.
- **GCP**: Cloud Identity directory, SSO/MFA, context-aware access, Workforce/Federation, IAM roles (basic/predefined/custom), service accounts/keys.

### Security & Compliance (high level)
- All support enterprise identity standards (SAML 2.0, OIDC, SCIM), MFA, conditional/context-aware access, and major compliance frameworks (ISO/SOC). Advanced features like **PIM** (Entra) and **Access Analyzer** (AWS) improve least privilege. Zero-trust models supported across suites.

### Pricing Model
- **Entra ID**: Free tier + Premium P1/P2 for Conditional Access, PIM, Identity Protection.
- **AWS IAM/Identity Center**: No direct cost for IAM; Identity Center typically no extra charge; you pay for underlying services (e.g., directory choices) and MFA methods if external.
- **GCP Cloud Identity/IAM**: Cloud Identity Free and Premium tiers; IAM itself has no direct charge.

### Integration for DevSecOps
- Works with CI/CD systems for OIDC workflows and short-lived tokens:
  - **Entra**: Workload identities, federated credentials for GitHub Actions/Azure DevOps; RBAC on ARM.
  - **AWS**: OIDC federation to GitHub/IDPs, role assumption via STS; IAM roles for service accounts (IRSA) on EKS.
  - **GCP**: Workforce/Federated identities, Workload Identity Federation for CI/CD; service accounts on GKE.

---

## 3) Azure Monitor vs Amazon CloudWatch vs Google Cloud Monitoring

### Overview
- **Azure Monitor**: Unified telemetry for Azure/on‑prem/other clouds; metrics, logs, traces, alerts, Insights, and action groups.
- **Amazon CloudWatch**: Metrics, logs, alarms, dashboards, events, and synthetics across AWS resources and apps.
- **Google Cloud Monitoring**: Part of the Operations suite (formerly Stackdriver) for metrics, uptime checks, dashboards, alerts.

### Core Features
- **Azure Monitor**: Platform metrics, activity logs, Application Insights, Workbooks, Alerts/Action Groups, autoscale, Synthetics (Availability tests).
- **CloudWatch**: Metrics/Logs/Alarms, Logs Insights queries, dashboards, Events/EventBridge, Synthetics canaries, Contributor Insights.
- **GCP Monitoring**: Metrics explorer, alerting policies, uptime checks, SLOs/error budgets, dashboards, Notification channels.

### Security & Compliance
- Data encryption at rest/in transit; role-based controls (Azure RBAC, AWS IAM, GCP IAM). Cross‑service compliance depends on region/service. Supports private endpoints/VPC flows where applicable.

### Pricing Model (general patterns)
- **Azure Monitor**: Charges for metric series beyond free grants; logs/ingestion/retention; Application Insights transactions.
- **CloudWatch**: Charges per metric, log ingest/storage, alarms, dashboards, synthetics.
- **GCP Monitoring**: Free allotments; charges for custom metrics and additional usage (Operations suite quotas).

### Integration for DevSecOps
- **Azure**: Alerts → **Logic Apps**, Functions; export via **Event Hubs**; GitHub/Azure DevOps dashboards.
- **AWS**: Alarms → SNS/Lambda/EventBridge; integrates with Code* services; observability with X-Ray.
- **GCP**: Alerting → Pub/Sub/Cloud Functions/Cloud Run; integrates with Cloud Build and Error Reporting/Trace/Profiler.

---

## 4) Azure Log Analytics vs CloudWatch Logs/Logs Insights vs Google Cloud Logging (+ BigQuery)

### Overview
- **Azure Log Analytics**: Central log store and query engine (KQL) within Azure Monitor; used by Sentinel and Defender for analytics/alerts/dashboards.
- **AWS CloudWatch Logs & Logs Insights**: Central log collection with on-demand query capabilities; supports subscriptions to Kinesis/Firehose/S3.
- **Google Cloud Logging**: Centralized logs with **Logs Explorer**; sinks to **BigQuery**, **Pub/Sub**, **Cloud Storage** for analysis/retention.

### Core Features
- **Log Analytics**: KQL queries, tables/schemas, workspaces, DCRs/Data Collection Rules, Solutions/Insights, scheduled queries/alerts.
- **AWS**: Log groups/streams, **Logs Insights** query language, metric filters, subscriptions, cross-account aggregation.
- **GCP**: Logs-based metrics, routing/sinks, Log Analytics in BigQuery, advanced analysis via SQL/Looker Studio.

### Security & Compliance
- Fine-grained access via RBAC/IAM; CMEK options vary by service; private networking options; regional residency options depend on provider. Widely used for audit, security, and compliance reporting.

### Pricing Model
- **Azure**: Pay‑as‑you‑go ingestion (GB), retention tiers, and query charges in some modes; Commitment Tiers available.
- **AWS**: Ingestion and storage priced per GB; queries for Logs Insights billed by data scanned.
- **GCP**: Ingestion within free tier then charged; BigQuery storage and query (per TB scanned) when exporting to BQ.

### Integration for DevSecOps
- **Azure**: Feeds **Microsoft Sentinel**, alerts/automation via Logic Apps/Functions, Event Hub export, Change Analysis.
- **AWS**: Feeds Security Hub/GuardDuty/Lambda analytics; export to S3 + Athena/Glue for further analysis.
- **GCP**: Sinks to **BigQuery** for SIEM-like analytics; triggers via Pub/Sub → Cloud Functions/Run; Chronicle (Google’s SIEM) optional.

---

## 5) Azure Policy vs AWS Config/SCPs vs GCP Organization Policy Service

### Overview
- **Azure Policy**: Governance engine to **audit, deny, and remediate** resource configurations at scale (subscription/management group/tenant). Supports initiatives and remediation tasks.
- **AWS**: **AWS Config** records/evaluates resource state; **Config Rules** automate checks; **Organizations SCPs** enforce **allow/deny** guardrails across accounts.
- **GCP Organization Policy Service**: Central constraints (allow/deny, conditions) across folders/projects; **Policy Controller/Config Validator** enforces Kubernetes/GKE and KRM policies.

### Core Features
- **Azure Policy**: Built‑in/custom policies, **deny/deployIfNotExists**, remediation, policy exemptions, versioned initiatives, Guest Configuration, Policy Insights.
- **AWS**: Resource inventory, conformance packs, managed/custom rules (Lambda), drift detection; **SCPs** for account‑level guardrails.
- **GCP**: Organization constraints, conditional policies, centralized inheritance; **Policy Controller** (OPA/Gatekeeper) for K8s; Terraform Validator for pre‑deploy checks.

### Security & Compliance
- Designed to enforce compliance frameworks (CIS/NIST/PCI) via built‑ins or packs; evidence via evaluation results. Works with each cloud’s audit/monitoring (Activity Logs, CloudTrail, Admin Activity Logs).

### Pricing Model
- **Azure Policy**: No charge for evaluation; certain features (e.g., Guest Configuration) may incur costs (e.g., Azure Automation/Arc). Remediation may deploy billable resources.
- **AWS Config**: Charged per configuration item, rule evaluations, conformance packs.
- **GCP Organization Policy**: No direct charge; enforcement is part of resource management (Ops costs for Policy Controller/GKE if used).

### Integration for DevSecOps
- **Azure**: Policy-as-code in pipelines, **deny‑by‑default**, **Blueprints/ALZ** integration, auto‑remediation via **Logic Apps** or deployIfNotExists.
- **AWS**: Conformance packs with CodePipeline; policy gates via SCPs; custom rules in Lambda; notifications via EventBridge.
- **GCP**: Policy testing in CI with **Config Validator**; GitOps with **Policy Controller**; constraints validated in Terraform/Cloud Build.

---

## 📌 Narrative Analysis & Recommendations

- For **IaC**, Azure **ARM/Bicep** and AWS **CloudFormation/CDK** are first‑class; in **GCP**, prefer **Config Connector** (GitOps) or **Terraform** for breadth.
- For **Identity**, all three deliver enterprise SSO/MFA/federation. If you need **PIM** and deep device compliance, **Entra ID P2** is strong. AWS excels in cross‑account role modeling; GCP’s **Workload Identity Federation** is excellent for keyless CI/CD.
- For **Observability**, parity is high. Choose based on your app stack and where you need to centralize data (e.g., **Log Analytics + Sentinel** vs **CloudWatch + Security Hub/GuardDuty** vs **Cloud Logging + Chronicle/BigQuery**).
- For **Governance**, combine **policy** + **posture management**. Azure **Policy** pairs well with **Defender for Cloud**; AWS uses **Config/Control Tower/SCPs**; GCP relies on **Org Policy** and **Security Command Center** (not covered here) plus **Policy Controller**.
- **Cost** considerations often hinge on **log ingestion/retention** and **custom metrics**. Establish routing and retention policies early (tiered storage, exclusions, and sampling).

---

## How to Use This in Your Repo

- Save this file as `README.md` at the root of your GitHub project.
- Add links to your lab artifacts (e.g., KQL queries, policy definitions, IaC templates).
- Consider adding a quickstart section showing how your team enforces policies and routes logs in your chosen cloud.

