
# Cloud Service Comparisons: Azure vs AWS vs GCP (Security, Ops, and DevSecOps)

This document compares **18 Azure services** used in whole course with their closest **AWS** and **GCP** counterparts.  
For each service you’ll find:
- **Overview**
- **Core Features**
- **Security & Compliance**
- **Pricing Model**
- **Integration for DevSecOps** (automation, CI/CD, monitoring)

---

## 🔎 Quick Mapping Table

| # | Azure | AWS | GCP |
|---|---|---|---|
| 1 | Azure Resource Manager (ARM) | CloudFormation / CDK | Deployment Manager (legacy) / Config Connector / Terraform |
| 2 | Microsoft Entra ID (Azure AD) | IAM + IAM Identity Center | Cloud Identity + Cloud IAM |
| 3 | Azure Monitor | CloudWatch | Cloud Monitoring |
| 4 | Azure Log Analytics | CloudWatch Logs + Logs Insights | Cloud Logging (+ BigQuery) |
| 5 | Azure Policy | AWS Config + SCPs | Organization Policy + Policy Controller |
| 6 | Azure Landing Zone | Control Tower | Landing Zone Blueprints/Toolkit |
| 7 | Microsoft Defender for Cloud | Security Hub + GuardDuty + Inspector | Security Command Center |
| 8 | Microsoft Sentinel | Security Hub + Detective (partial) | Chronicle Security Operations |
| 9 | Azure Logic Apps | Step Functions + EventBridge (+ Lambda) | Workflows + Eventarc |
| 10 | Azure Functions | Lambda | Cloud Functions |
| 11 | Azure Event Hubs | Kinesis Data Streams / MSK | Pub/Sub |
| 12 | Azure Storage (Blob) | S3 | Cloud Storage |
| 13 | Azure Key Vault | Secrets Manager + KMS | Secret Manager + Cloud KMS |
| 14 | Azure Firewall | AWS Network Firewall | Cloud Firewall |
| 15 | Network Security Groups (NSG) | Security Groups | VPC Firewall Rules |
| 16 | Azure Blueprints | Service Catalog + Control Tower | Deployment Manager/Terraform + Org Policy bundles |
| 17 | Azure DevOps | CodePipeline/Build/Commit/Deploy | Cloud Build + Artifact Registry + Cloud Deploy |
| 18 | Azure API Management | API Gateway | Cloud Endpoints / API Gateway |

---

## 1) Azure Resource Manager (ARM) ↔ CloudFormation/CDK ↔ Deployment Manager/Config Connector

**Overview**  
- **ARM:** Azure’s control plane & IaC engine using Bicep/JSON; declarative deployments and scoped RBAC.
- **CloudFormation/CDK:** Declarative stacks (YAML/JSON) or CDK (TypeScript/Python, etc.) to synthesize templates.
- **GCP Config Connector:** Kubernetes-native CRDs for GCP resources; Deployment Manager is legacy; Terraform widely used.

**Core Features**  
- **ARM:** Bicep, what-if, deployment stacks, template specs, tenant/subscription/RG scopes.  
- **CloudFormation:** Change sets, drift detection, StackSets (multi-account/region), modules, CDK constructs.  
- **GCP:** GitOps via Config Connector/Config Sync; KRM policy; Terraform ecosystem.

**Security & Compliance**  
Common baselines: ISO 27001, SOC 1/2/3, PCI DSS (service-dependent), HIPAA eligible offerings, FedRAMP (selected regions). RBAC/IAM on control plane, private endpoints/VPC SC where applicable.

**Pricing Model**  
No direct charge for ARM/CFn/DM/Config Connector; pay for provisioned resources and any underlying orchestration (e.g., GKE/EKS cluster cost for KRM controllers).

**Integration for DevSecOps**  
- **ARM:** Azure DevOps/GitHub Actions; policy-as-code with Azure Policy; linting (Bicep analyzers).  
- **AWS:** CodePipeline/CodeBuild; guardrails via SCPs/Config; cdk-nag checks.  
- **GCP:** Cloud Build/GitOps; Policy Controller; Terraform Validator in CI.

---

## 2) Microsoft Entra ID (Azure AD) ↔ IAM + Identity Center ↔ Cloud Identity + IAM

**Overview**  
- **Entra ID:** Cloud IdP for users/apps/devices; SSO, MFA, Conditional Access, B2B/B2C.  
- **AWS IAM + Identity Center:** IAM for policies/roles/STSa; Identity Center for SSO across accounts/apps.  
- **GCP Cloud Identity + IAM:** Directory/SSO/MFA + resource-level IAM roles.

**Core Features**  
- **Entra:** SAML/OIDC, Conditional Access, PIM, risk-based identity protection, SCIM provisioning.  
- **AWS:** Fine-grained policies, federation (SAML/OIDC), permission sets, Access Analyzer.  
- **GCP:** Context-aware access, workforce/workload identity federation, predefined/custom roles, service accounts.

**Security & Compliance**  
Supports enterprise auth standards; strong MFA; major certifications (ISO/SOC). PIM (Entra P2) and Access Analyzer (AWS) enhance least privilege; keyless CI tokens supported across clouds.

**Pricing Model**  
Entra: Free, P1, P2 tiers. IAM/Identity Center: no direct cost (directory/MFA extras may apply). Cloud Identity: Free & Premium; Cloud IAM no direct charge.

**Integration for DevSecOps**  
OIDC federation for CI/CD (GitHub/Azure DevOps/Cloud Build); short‑lived tokens; workload identities for K8s/serverless.

---

## 3) Azure Monitor ↔ CloudWatch ↔ Cloud Monitoring

**Overview**  
Unified telemetry (metrics, logs, traces, alerts) for infra and apps.

**Core Features**  
- **Azure Monitor:** Platform metrics, Activity Logs, Application Insights, Workbooks, Action Groups, autoscale.  
- **CloudWatch:** Metrics/Logs/Alarms, Logs Insights, Synthetics, EventBridge, Contributor Insights.  
- **GCP:** Metrics explorer, uptime checks, SLOs/error budgets, alerting policies.

**Security & Compliance**  
Encrypted in transit/at rest; RBAC/IAM to data; private networking options; regional storage controls.

**Pricing Model**  
Charges for custom metrics, log ingestion/retention, queries, synthetics. Free grants vary by cloud.

**Integration for DevSecOps**  
Alerts to automation (Logic Apps/Lambda/Cloud Functions), export to buses (Event Hubs/EventBridge/Pub/Sub), dashboards in CI/CD.

---

## 4) Azure Log Analytics ↔ CloudWatch Logs/Insights ↔ Cloud Logging (+ BigQuery)

**Overview**  
Central log store & query engine (KQL for Azure; Logs Insights for AWS; Logs Explorer & BigQuery for GCP).

**Core Features**  
Workspaces/log groups, powerful queries, scheduled alerts, routing/exports, logs-based metrics (GCP), schema-on-read analytics (BigQuery/Athena).

**Security & Compliance**  
Role-scoped access; CMEK options; retention controls; audit trails.

**Pricing Model**  
Ingestion per-GB, retention/storage, query charges (data scanned). Commit tiers on Azure; tiered pricing on AWS/GCP.

**Integration for DevSecOps**  
Feeds SIEM (Sentinel/Chronicle/Security Hub), triggers playbooks/functions, integrates with IaC scans and policy insights.

---

## 5) Azure Policy ↔ AWS Config + SCPs ↔ Organization Policy + Policy Controller

**Overview**  
Policy engines to audit/deny/remediate resource configurations.

**Core Features**  
- **Azure:** Built‑in/custom policies, initiatives, deployIfNotExists, Guest Configuration, remediation tasks.  
- **AWS:** Resource inventory, conformance packs, managed/custom rules (Lambda), SCPs for guardrails.  
- **GCP:** Org constraints, conditional policies, KRM policy controller (OPA/Gatekeeper), Terraform Validator.

**Security & Compliance**  
Implements CIS/NIST/PCI mappings, evidence via evaluation results and dashboards (Policy Insights/Config/Asset Inventory).

**Pricing Model**  
Azure Policy eval free; remediation resources may incur cost. AWS Config billed per config item/rule. GCP Org Policy free; Policy Controller runs on GKE (cluster cost).

**Integration for DevSecOps**  
Policy-as-code in pipelines; pre-commit/pre-deploy validation; auto-remediation via workflows (Logic Apps/Lambda/Cloud Run).

---

## 6) Azure Landing Zone ↔ Control Tower ↔ GCP Landing Zone Blueprints/Toolkit

**Overview**  
Foundational multi‑subscription/account/project setup with identity, networking, security, and governance baselines.

**Core Features**  
Reference architectures, guardrails/policies, logging/monitoring, centralized identity, cost/tag standards, automation modules.

**Security & Compliance**  
Baseline controls aligned to CIS/NIST/FedRAMP reference guidance (varies by provider packages).

**Pricing Model**  
No direct charge for patterns; you pay for deployed shared services (log archives, security accounts, networking hubs).

**Integration for DevSecOps**  
Bootstraps IaC repos, policy-as-code, central logging/alerts, and account vending automation.

---

## 7) Microsoft Defender for Cloud ↔ Security Hub + GuardDuty + Inspector ↔ Security Command Center

**Overview**  
Cloud-native application protection (CNAPP): posture, threat detection, and workload protection.

**Core Features**  
- **Azure:** CSPM, CWPP, DevOps security (IaC scanning), recommendations, just‑in‑time, agent-based/less modes.  
- **AWS:** Security Hub (findings aggregation), GuardDuty (threat detection), Inspector (vuln).  
- **GCP:** SCC (findings, posture, threat detection; Premium adds advanced features).

**Security & Compliance**  
Maps to CIS/NIST/PCI; integrates with IAM/Policy/Logging; supports evidence and workflows.

**Pricing Model**  
Per‑resource or per‑vCPU/GB/month style; tiered (Free/Standard/Premium equivalents). Costs vary by sensors and data volume.

**Integration for DevSecOps**  
Feeds SIEM; ticketing and playbooks; IaC scanning in CI; auto-remediation hooks (Logic Apps/Lambda/Cloud Functions).

---

## 8) Microsoft Sentinel ↔ AWS Security Hub + Detective (partial) ↔ Chronicle Security Operations

**Overview**  
SIEM/SOAR for threat detection, hunting, and automated response.

**Core Features**  
- **Sentinel:** KQL analytics, workbooks, built-in connectors, playbooks (Logic Apps), UEBA, hunting, fusion ML.  
- **AWS:** No first‑party SIEM; Security Hub aggregates, **Detective** assists investigations; partners for SIEM.  
- **GCP:** Chronicle SecOps (SIEM + SOAR), high‑scale ingestion, detections content, case mgmt.

**Security & Compliance**  
Enterprise certifications across platforms; data residency and CMEK options vary by product.

**Pricing Model**  
Sentinel/Chronicle priced largely on data ingestion/storage; AWS components priced per feature (Hub/Detective).

**Integration for DevSecOps**  
CI‑triggered detections testing, SOAR playbooks, ticketing integration, purple-team hunting content in repos.

---

## 9) Azure Logic Apps ↔ Step Functions + EventBridge (+ Lambda) ↔ Workflows + Eventarc

**Overview**  
Serverless orchestration/automation (low-code).

**Core Features**  
Connectors, state machines, retries, schedules, error handling; event routing (EventBridge/Eventarc).

**Security & Compliance**  
Managed identities/service roles, private networking, encryption options, enterprise certs.

**Pricing Model**  
Per-action/execution pricing; connectors/premium plans may differ; event egress/ingress billed separately.

**Integration for DevSecOps**  
Automate ops runbooks, remediation, change approvals; pipeline hooks for change management.

---

## 10) Azure Functions ↔ Lambda ↔ Cloud Functions

**Overview**  
Event-driven serverless compute.

**Core Features**  
Triggers/bindings (HTTP, queues, blobs), autoscale, extensions; languages vary per provider.

**Security & Compliance**  
Managed identities/roles; VNET/VPC access; secrets from vaults/secrets mgr; major certifications.

**Pricing Model**  
Requests + GB‑seconds + optional provisioned concurrency. Free tiers exist.

**Integration for DevSecOps**  
CI/CD (GitHub Actions/CodePipeline/Cloud Build), IaC deployment, can run policy checks and responders.

---

## 11) Azure Event Hubs ↔ Kinesis/MSK ↔ Pub/Sub

**Overview**  
High-throughput event ingestion and streaming.

**Core Features**  
Partitions, consumer groups, capture/export, ordered at‑least‑once delivery; Kafka compatibility (Event Hubs for Kafka, MSK).

**Security & Compliance**  
Private endpoints/VPC, SAS/IAM auth, encryption, enterprise certs.

**Pricing Model**  
Throughput units/Capacity units (Azure/Kinesis), per‑partition and data volume; MSK charges for cluster resources. Pub/Sub bills per message/egress/retention.

**Integration for DevSecOps**  
Hooks for audit streams, telemetry pipelines, and real‑time detections (to Functions/Lambda/Cloud Functions).

---

## 12) Azure Storage (Blob) ↔ S3 ↔ Cloud Storage

**Overview**  
Object storage for unstructured data.

**Core Features**  
Tiers (hot/cool/archive), lifecycle mgmt, versioning, immutability (WORM), event notifications, static website hosting.

**Security & Compliance**  
CMEK, immutability, bucket/container policies, private networking, enterprise certs; data residency choices.

**Pricing Model**  
Per‑GB storage, requests, egress; lifecycle to colder tiers to reduce cost.

**Integration for DevSecOps**  
Artifact storage, logs/archive, policy compliance scans, event-driven workflows.

---

## 13) Azure Key Vault ↔ Secrets Manager + KMS ↔ Secret Manager + Cloud KMS

**Overview**  
Secrets, keys, and certificates management.

**Core Features**  
Versioning, rotation, RBAC/IAM, HSM-backed keys, APIs/SDKs, auditing.

**Security & Compliance**  
FIPS 140‑2/3 HSM tiers (service/region dependent), RBAC/IAM, VPC endpoints/private link, enterprise certs.

**Pricing Model**  
Operations per 10K calls, key versions, HSM vs software tier; rotation and private link can add cost.

**Integration for DevSecOps**  
Inject secrets into CI/CD, serverless, containers; sign/verify artifacts; encrypt data at rest and in transit with CMEK.

---

## 14) Azure Firewall ↔ AWS Network Firewall ↔ Cloud Firewall

**Overview**  
Managed, scalable L3–L7 firewall with advanced threat protection (varies by product).

**Core Features**  
Stateless/stateful rules, FQDN/IP lists, TLS inspection (offerings vary), IDS/IPS integrations, central policy mgmt.

**Security & Compliance**  
Meets enterprise networking/security standards; logging to SIEM; private/hub-spoke deployments.

**Pricing Model**  
Hourly gateway + data processed; rules/threat intelligence add-ons may apply.

**Integration for DevSecOps**  
IaC for rulesets; pipelines for policy promotion; logs feed SIEM.

---

## 15) Network Security Groups (NSG) ↔ Security Groups ↔ VPC Firewall Rules

**Overview**  
Instance/subnet-level virtual firewall rules.

**Core Features**  
Allow/deny, inbound/outbound, priorities, tags/service references, default denies.

**Security & Compliance**  
Least-privilege segmentation; logs to flow logs; policy guardrails (Policy/SCP/Org Policy).

**Pricing Model**  
No direct charge; pay for flow logs/analytics if enabled.

**Integration for DevSecOps**  
Rules as code; drift detection; policy enforcement in CI.

---

## 16) Azure Blueprints ↔ Service Catalog + Control Tower ↔ Deployment Manager/Terraform + Org Policy

**Overview**  
Package IaC, policies, and RBAC as environment blueprints for consistent provisioning.

**Core Features**  
Artifacts (ARM/Bicep), policy assignments, RBAC, resource hierarchies; lifecycle/versioning.

**Security & Compliance**  
Standardized environments comply with org controls; audit via policy/activities.

**Pricing Model**  
No direct charge; cost is from deployed resources.

**Integration for DevSecOps**  
Environment vending pipelines, guardrails baked into templates, PR-based approvals.

---

## 17) Azure DevOps ↔ CodePipeline/Build/Commit/Deploy ↔ Cloud Build + Artifact Registry + Cloud Deploy

**Overview**  
CI/CD and project management tooling (boards, repos, pipelines, test plans, artifacts).

**Core Features**  
Hosted agents, YAML pipelines, gated releases, artifacts feeds, approvals, multi-stage deployments.

**Security & Compliance**  
RBAC, secret stores, SSO, audit logs; compliance attestations vary by plan/region.

**Pricing Model**  
Free tiers; billed users/parallel jobs/storage; similar consumption models across providers.

**Integration for DevSecOps**  
Security scanning in pipelines (SAST/DAST/IaC), SBOM, policy checks, deployment gates, observability hooks.

---

## 18) Azure API Management ↔ API Gateway ↔ Cloud Endpoints/API Gateway

**Overview**  
Managed API gateway for publishing, securing, and monitoring APIs.

**Core Features**  
Auth (OAuth2/JWT), throttling, quotas, transformations, versioning, developer portal, analytics; hybrid/self-hosted gateways available (vendor-specific).

**Security & Compliance**  
mTLS/JWT validation, WAF integrations, private endpoints, enterprise certifications; audit logging to SIEM.

**Pricing Model**  
Per-hour/instance or calls-based tiers; data transfer adds cost; developer vs production sku differences.

**Integration for DevSecOps**  
API-as-code, policy promotion via CI/CD, contract testing, canary releases, telemetry to monitoring/siem.

---
