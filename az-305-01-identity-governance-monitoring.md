# AZ-305: Identity, Governance, and Monitoring

All notes taken from [the relevant AZ-305 learning path](https://learn.microsoft.com/en-us/training/paths/design-identity-governance-monitor-solutions/) starting point.

## Governance

- Hierarchy

  - **Tenant root group** = container, allowing global policies and roles
  - **Management Groups** (6 levels) = manage access, policy, compliance
    - limit regions for VM's
    - role assignments to be inherited by subscriptions
    - monitor and audit
  - **Subscriptions** = logical grouping, billing boundary
    - Consider a subscription per DTAP environment
    - Virtual Networks can't be shared across subscriptions
  - **Resource Groups** = logical grouping, _metadata_ in one region
    - Cannot be nested
    - Cannot be renamed
    - Grouped creation or deletion of resources
    - Manage permissions via Groups
    - Allows Azure policies, Azure roles
    - Tags possible, not inherited by resources in group
  - **Resources** = instances of services
    - Each resource is in exactly one resource group
    - Tags = name-value pairs

- **Strategies**

  - Azure policies
    - Control and/or audit resources
    - Inherited through hierarchy
    - Works for Azure Arc (e.g. VMware vSphere, Kubernetes clusters)
    - Can automatically remediate
    - To be combined with RBAC
  - **Role-Based Access Control (RBAC)**
    - See [flowchart](./assets/images/az-305-governance-rbac-flowchart.png)
  - **Resource locks** (override user permissions)
    - Applicable for
      - Subscription
      - Resource group
      - Resource
    - Variants
      - CanNotDelete
      - ReadOnly

- **Azure Landing Zone** = Architectural decision and design

  - Types: Application Landing Zones Platform Landing Zones
  - Accelerator =n portal-based deployment experience

## Authentication and Authorization

- **Identity and Access Management (IAM)**

  - Microsoft Entra ID basics
    - Multi-tenant
    - Cloud-based directory services
    - Application access management
    - Identity protection
    - Cloud-only vs Hybrid (extending on-premises Active Directory)
      - Microsoft Entra Connect
      - Microsoft Entra Connect cloud sync

- **Microsoft Entra B2B**

  - Invite External Identities as Guest users
  - Conditional Access policies based on extra factors
  - Optionally enable MFA

- **Azure AD B2C** (a "type of Microsoft Entra tenant, separate from the Microsoft Entra Tenant)

  - Custom attributes for customers
  - Custom sign-in and sign-up experience
  - User flows support
  - Optionally store users externally

- **Conditional Access** (requires P1 or P2 license)

  - Actions: allow or deny access, enforce MFA
  - Conditions: user/group, cloud app, device state, location (ip range), client app, sign-in risk
  - Report-only mode

- **Identity Protection**

  - Automate detection and remediation
  - Investigate risks
  - Export risk detection data
  - Risk policies
    - User risk (is account compromised?)
    - Sign-in risk (suspicious sign-in?)

- **Access Review**

  - Check if access is still needed by
    - Resource (business) owners
    - Delegates that do the reviews
    - End user (self-attest)

- **Security Principals**

  - User Principal
  - Service Principal (for app instances) with Application Objects ("definition" for an app)
    - Application Service Principal = local representation in a specific tenant
    - Managed Identity (with Client ID and Object ID)
    - Legacy Service Principal (might have credentials)

- **Managed Identities**

  - Types
    - System-assigned (automatically created by Azure for your resource)
    - User-assigned (by an admin, can be assigned to multiple instances of an Azure service)
  - Combine with Azure Key Vault

- **Azure Key Vault**

  - Manage secrets, keys, certificates
  - Tiers
    - Standard: encrypted with software key
    - Premium: encrypted with hardware security module (HSM) keys
  - Separate (e.g. by application is most common)
  - Soft delete and Purge protection

## Log and Monitor Resources

- **Azure Monitor**

  - Logs and Metrics
  - Data Sources
    - Application data (custom app code)
    - Operating System data (diagnostics, or via monitor logs agent / dependency agent)
    - Azure resources data (e.g. web app or load balancer)
    - Subscription data and Azure health
    - Tenant data and AD
  - Query logs
  - Alerts based on Logs and Metrics

- **Azure Monitor Log Analytics workspaces**

  - Tables for organization
  - Pricing tier, retention, data capping
  - Access modes
    - **Workspace-context**: user can review all logs in workspace
    - **Resource-context**: user can review logs only for tables (and rows) scoped to their resource
  - **Azure Workbooks** - homebrew prefab analysis
    - Combine sources
    - Supports: Logs, Metrics, Resource Graph, Alerts, Workload Health, Resource Health, Data Explorer
  - **Azure insights** - custom monitoring
    - Customized monitoring experience
    - Features of Azure Monitor, e.g.
      - Application Insights (APM for web apps)
      - Container Insights (e.g. AKS clusters)
      - Network Insights
      - Resource Group Insights
      - VM Insights
      - etc.

- **Microsoft Sentinel**

  - Security Information and Event Management (SIEM)
  - Security Orchestration, Automation, and Response (SOAR)

- **Azure Data Explorer**

  - Parts
    - Azure Data Explorer Unified Analytics Platform
    - Azure Data Explorer SDKs
    - Azure Data Explorer Web UI
  - Integrates with Databricks and Azure ML
  - Long data retention
