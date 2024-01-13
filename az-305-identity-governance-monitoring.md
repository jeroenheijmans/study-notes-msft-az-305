# AZ-305: Design identity, governance, and monitor solutions

All notes taken from [the relevant AZ-305 learning path](https://learn.microsoft.com/en-us/training/paths/design-identity-governance-monitor-solutions/) starting point.

- **Governance**
  - Hierarchy
    - **Tenant root group** = container, allowing global policies and roles
    - **Management Groups** (6 levels) = manage access, policy, compliance
      - limit regions for VM's
      - role assignments to be inherited by subscriptions
      - monitor and audit
    - **Subscriptions** = logical grouping, billing boundary
      - Consider a subscription per DTAP environment
      - Virtual Networks can't be shared across subscriptions
    - **Resource Groups** = logical grouping, *metadata* in one region
      - Cannot be nested
      - Cannot be renamed
      - Grouped creation or deletion of resources
      - Manage permissions via Groups
      - Allows Azure policies, Azure roles
      - Tags possible, not inherited by resources in group
    - **Resources** = instances of services
      - Each resource is in exactly one resource group
      - Tags = name-value pairs
  - Strategies
    - Azure policies
      - Control and/or audit resources
      - Inherited through hierarchy
      - Works for Azure Arc (e.g. VMware vSphere, Kubernetes clusters)
      - Can automatically remediate
      - To be combined with RBAC
    - Role-Based Access Control (RBAC)
      - See [flowchart](./img/az-305-governance-rbac-flowchart.png)
    - Resource locks (override user permissions)
      - Subscription
      - Resource group
      - Resource
  - Azure Landing Zone = Architectural decision and design
    - Types: Application Landing Zones Platform Landing Zones
    - Accelerator =n portal-based deployment experience
