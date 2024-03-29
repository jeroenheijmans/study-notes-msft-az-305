# AZ-305: Prerequisites

All notes taken from [the relevant AZ-305 learning path](https://learn.microsoft.com/en-us/training/paths/microsoft-azure-architect-design-prerequisites/) starting point.

## Core Architectural Components

- **Azure Physical Infrastructure**

  - Regions contain 1 or more Datacenters
  - Availability zones have physically separate Datacenters within a Region
  - Region Pairs of datacenters 300+ miles apart
  - Sovereign Regions are for US Government and China

- **Azure Account**

  - **Account**
    - Needed to do anything on Azure
    - Requires at least 1 subscription
  - **Subscriptions**
    - Unit of management, billing, and scale
    - Boundaries
      - Billing boundary
      - Access control boundary
    - Typical usages
      - DTAP
      - Organizational structure
      - Billing purposes
  - **Resource Groups**
    - Purely for grouping resources
  - **Resources**
    - Are individual services, VM's, etc.
    - Always belong to 1 Resource Group
  - **Management group**
    - Hierarchy to apply policies for governance
    - User access to multiple subscriptions

## Azure Compute and Networking

- **Virtual Machines**

  - **Virtual Machine Scale Set**
    - Group of identical, load-balanced VM's
  - Virtual Machine Availability Set
    - Staggered updates and varied power/network connectivity
      - Update domain (VM's to be rebooted at the same time)
      - Fault domain (VM's will share power source and network switch)

- **Azure Virtual Desktop**

  - Cloud-hosted Windows desktop
  - Multi-session support
  - Isolated user sessions

- **Containers**

  - **Azure Container Instances**: fastest and simplest containers
  - **Azure Container Apps**: PaaS offering that can include load balancing and scaling
  - **Azure Kubernetes Service (AKS)**: orchestrated containers

- **Azure Functions**

  - Serverless, event-driven
  - Scale on demand
  - Stateless (default) or stateful

- **Application Hosting**

  - Azure App Service
  - Web apps (.NET, Java, Ruby, NodeJS, PHP, python - on Windows or Linux)
  - Background jobs
  - Mobile back-ends
  - REST APIs

- **Virtual Networking**

  - Isolation and segmentation: private IP address space
  - Name resolution from Azure or external DNS server
  - Internet connected
  - Azure connected through the network or service endpoints
  - Optionally connect to on-premises
  - Routing traffic
  - Filtering traffic
  - Connect multiple virtual networks (e.g. through peering)
  - _User-Defined Routes (UDR)_

- **Azure Virtual Private Networks** (VPNs)

  - **Azure VPN Gateway**
    - Connectivity
      - **Site-to-site**: on-premises datacenter
      - **Point-to-site**: individual device
      - **Network-to-network**: virtual network
    - Type
      - **Policy-based**: IP address based
      - _Route-based_: IPSec tunnels (preferred to connect on-premises devices)
    - High availability options
      - **Active/standby** (to account for maintenance in Azure)
      - **Active/active** (using BGP routing protocol, multiple active public IP's for gateway)
      - **ExpressRoute failover**
      - Zone-redundant gateways

- **Azure ExpressRoute**

  - Benefits
    - Connectivity to all cloud services
    - Global connectivity
    - Dynamic routing via BGP
    - Built-in redundancy
  - Models
    - CloudExchange colocation
    - Point-to-point Ethernet connection
    - Any-to-any connection
    - Directly from ExpressRoute sites

- **Azure DNS**

  - Supports private DNS domains
  - Supports Alias record sets

## Azure Storage Services

- **Storage Account**

  - Unique namespace
  - **Tiers**
    - Standard General Purpose v2: recommended for most cases
    - Premium block blobs: for high transaction rates and low storage latency
    - Premium file shares: for high performance SMB and NFS shares
    - Premium page blobs: for page blobs only
  - Services
    - Blobs (text and binary data)
      - **Tiers**
        - Hot: frequently accessed data (e.g. images)
        - Cool: infrequently accessed, stored at least 30 days (e.g. invoices)
        - Cold: infrequently accessed, stored at least 90 days
        - Archive: rarely accessed, stored at least 180 days
    - Files (for shares)
      - Azure File Sync to make Windows Servers cache the share
    - Queues (messaging)
      - Max 64KB
    - Disks (storage volumes for VM's)
    - Tables (NoSQL option)
  - File movement tools
    - AzCopy: upload, download, copy between storage accounts
    - Azure Storage Explorer
    - Azure File Sync

- **Redundancy options**

  - **Locally redundant storage (LRS)**
    - Replicate data 3 times in one data center in primary region
    - SLA: 11 nines (99.999999999%) of objects over a given year
  - **Zone-redundant storage (ZRS)**
    - Replicate synchronously across 3 availability zones in primary region
    - SLA: 12 nines (99.9999999999%) over a given year
  - **Geo-redundant storage (GRS)**
    - Similar to LRS in two regions
    - SLA: 16 nines (99.99999999999999%) over a given year
  - **Geo-zone-redundant storage (GZRS)**
    - Similar to ZRS in primary and LRS in secondary region
    - SLA: 16 nines (99.99999999999999%) of durability of objects over a given year
  - **Read-access geo-redundant storage (RA-GRS)**
  - **Read-access geo-zone-redundant storage (RA-GZRS)**

- **Azure Migrate**: see later modules in AZ-305

- **Azure Data Box**: see later modules in AZ-305

## Azure Identity, Access, and Security

- **Microsoft Entra ID**

  - Services
    - Authentication
    - Single sign-on (SSO)
    - Application Management
    - Device Management
  - **Microsoft Entra Connect**
    - Synchronize on-premises Active Directory

- **Microsoft Entra Domain Services**

  - Managed domain services
  - LDAP, Kerberos/NTLM
  - Helps with Lift & Shift of legacy
  - Integrates with existing Microsoft Entra tenant

- **External Identities**

  - B2B
  - B2B direct connect (two-way trust)
  - B2C

- **Conditional Access**

  - Granular multi-factor authentication
  - Purposes
    - Require MFA depending on user properties
    - Require access through specific clients
    - Requires access only from managed devices
    - Block access if suspicious

- **Role-based Access Control (RBAC)**

  - Allow model
  - Scopes
    - Management group
    - Subscription
    - Resource group
    - Resource

- Zero trust (no notes)

- Defense-in-depth (no notes)

- Microsoft Defender for Cloud (no notes)

## Microsoft Cloud Adoption Framework for Azure

See: [Infrastructure](./az-305-04-infrastructure.md)

## Microsoft Azure Well-Architected Framework

See: [Infrastructure](./az-305-04-infrastructure.md)
