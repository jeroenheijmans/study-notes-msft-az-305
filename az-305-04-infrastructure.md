# AZ-305: Infrastructure

All notes taken from [the relevant AZ-305 learning path](https://learn.microsoft.com/en-us/training/paths/design-infranstructure-solutions/) starting point.

## Azure Compute

- Overview (see [flowchart](./assets/images/az-305-infrastructure-compute-flowchart.png))

- **Azure VMs** (IaaS)

  - Lift and Shift Migrations without containerization
  - New workloads requiring full control
  - Classifications (tiers)
    - **General purpose**: balanced CPU-to-memory (test and dev, or small-to-medium scenarios)
    - **Compute optimized**: high CPU (web servers, network appliances, batch processes)
    - **Memory optimized**: high memory (relational database servers, caches, etc)
    - **Storage optimized**: high disk throughput and I/O (database servers)
    - **GPU**: graphics rendering and video editing, and e.g. deep learning
    - **High performance computes**: high CPU and optional high-throughput network (high performance scenarios)
  - Costs
    - Compute: priced hourly, billed per minute of use
    - Storage: charged for Storage used by disks
  - Operating system (64bit only)
    - Windows (OS license included)
    - Linux
    - Azure Marketplace for OS image with tools (e.g. WordPress)

- **Azure Batch** (PaaS)

  - Cloud optimized HPC workloads (up to thousands of VM's)
  - Good for parallel workloads
  - Good for tightly coupled workloads where apps need to communicate
  - Azure Storage to fetch applications or data
  - Azure Storage to write output
  - Can use pooling (allocate pools with multiple nodes and/or isolated VM sizes for guaranteed progress)
  - Optimize jobs and tasks
  - Has [best practices guidance](https://learn.microsoft.com/en-us/azure/batch/best-practices)

- **Azure App Service** (PaaS)

  - Cloud optimized HTTP-based service
  - For web apps, background jobs (WebJobs), mobile backends, REST APIs
  - Supports ASP.NET, ASP.NET Core, Java, Ruby, NodeJS, PHP, Python
  - Supports Windows or Linux host
  - Built-in load balancing and traffic management with high availability
  - Allows for [built-in auth ("Easy Auth")](https://learn.microsoft.com/en-us/azure/app-service/overview-authentication-authorization)
  - Deployment slots for DTAP (Standard App Service Plan or higher)
  - App Service Plan for costs

- **Azure Functions** (FaaS)

  - Event-driven workloads with short-lived processes (Code First)
  - C#, Java, JavaScript, PowerShell, Python
  - Avoid long-running functions
    - Consumption Plan: 300 second time-out
    - Other plans: 30 minutes time-out
  - Durable functions for state and function chaining
  - Group functions with different load profiles
  - Defensive coding needed
  - Separate storage account for each function application

- **Azure Logic Apps** (FaaS)

  - Event-driven workloads with short-lived processes (Design First)
  - Part of "Azure Integration Services"
  - Best if it has external connections
  - Might have slow activation time

- **Azure Container Instances** (PaaS)

  - Cloud-optimized microservices
  - Lift and shift if your workload can be containerized
  - Fast startup, per-second billing, persistent storage (e.g. via Azure Files shares)
  - Container Group (e.g. front-end container grouped with back-end, or app container with monitoring container)
  - Best practices
    - Private registry (Docker Trusted Registry or Azure Container Registry)

- **Azure Kubernetes Service (AKS)** (PaaS)

  - Full-fledged orchestration for cloud-optimized containerizable microservices
  - Container Management (organize and change containers)
  - Container Orchestration (dynamically adjust number of instances, automatically update running instances)
  - Scaling options
    - Horizontal pod autoscaler (to meet increased demand)
    - Cluster autoscaler (automatically scale cluster nodes)
  - Utilize ARM templates
    - networking
    - Microsoft Entra integration
    - monitoring
  - Supports Azure Monitor
  - Supports Virtual Networks
  - Supports static and dynamic storage volumes
  - Supports ingress with HTTP application routing
  - Supports Azure Container Registry

## Application Architecture

- **Messages**

  - Raw data itself in messages
  - Expectations between producer and consumer
  - Allows for guaranteed communication
  - **Azure Queue Storage**
    - Large numbers (millions) of messages in Azure Storage
    - Limited only by Azure Storage Account limits (e.g. over 80 GB)
    - Increased reliability
    - Guaranteed message delivery
    - Transactional support
  - **Azure Service Bus** (High-value enterprise messaging)
    - Message broker
    - Allows load-balancing work
    - High degree of reliability
    - Under 80 GB queue storage
    - Under 64 KB message payload total size (32 KB per property)
    - Allows FIFO guarantee (with "sessions")
    - Supports "Auto-forwarding"
    - Supports Dead-lettering
    - Message Queues
      - Message broker on top of Service Bus
      - Intended for Enterprise Applications
    - Publish-Subscribe Topics
      - Multiple subscribers possible
      - Each subscriber receives a copy of the message

- **Events**

  - Light-weight broadcasts
  - Publish / Subscribe model
  - Zero to many subscribers possible
  - **Azure Event Hubs** (Big Data Pipeline)
    - Millions of events per second possible
    - Real-time as well as store for later analysis
    - Integrates with Apache Storm
    - Events ordered by receive time, consumers can seek along the stream
    - Events are kept (until time-o-live expired) even when consumed
    - No Dead Letter or other similar mechanism
    - Pricing
      - Scaling through "purchased throughput units"
        - Ingress unit = 1MB per second OR 1000 events per second
        - Egress unit = 2MB per second OR 4096 events per second
      - Performance per tier (Basic, Standard, Premium)
    - Ideal for: live dashboarding, supporting clickstreams, detecting anomalies (fraud, or outliers), processing transactions with real-time analysis
    - Supports many programming languages
  - **Azure Event Grid** (Reactive programming)
    - Runs on Azure Service Fabric
    - Sources like Azure Blog Storage or Azure Media Services
    - Targets like Azure Functions and Azure DevOps Webhooks
    - Events contain reference (URL or identifier) to subject
  - **Azure IoT Hub**
    - Fully managed service
    - Bi-directional communications between millions of devices
    - Secure communications
    - Hyper-scale communication options
    - Messaging patterns
      - Device-to-cloud telemetry
      - Uploading files from devices
      - Request-reply methods to control devices from the cloud
      - Device creation, connection, and failure tracking

- **Caching**

  - **Azure Cache for Redis**
    - Low-latency, high-throughput
    - Either Redis Open Source (OSS Redis) or Redis Enterprise (managed)
    - Multiple functions
      - Data cache (cache-aside pattern to load data into cache as needed, with expirations or eviction policies)
      - Content cache (for quick access to static content)
      - Session store (e.g. user sessions)
      - Message broker (supports distributed queue)
      - Distributed transactions (batch of commands in single transaction)

- **Azure API Management**

  - Suitable when you have
    - Large number of APIs
    - High rate of API changes
    - High API administration load (e.g. policies for quotas, rate limits, request transformations and validation)
    - Need to standardize disparate APIs
    - Need for enhanced API security

- **App Deployments**

  - **Azure Resource Manager (ARM) templates**
    - Declarative approach
    - Idempotent
    - Deployments in parallel
    - Supports "WhatIf"
    - Validated before deployment
    - Integrates with CI/CD tools
    - Different formats
      - JSON templates
      - Bicep templates (DSL for ARM templates)
  - **Azure Automation**
    - Cloud-based automation and configuration
      - Process automation (automate time-consuming, error-prone cloud management tasks)
      - Configuration Management (change tracking)
      - Update management (create scheduled deployments in maintenance windows)

- **App Configuration**

  - **Azure App Configuration**
    - Central management of Application Settings and Feature Flags
    - Fully managed
    - Tagging with labels, key representations and mappings
    - Dedicated UI for feature flag management
    - Encryption at rest and in transit
    - Complements Azure Key Vault
    - Poll for changes or use Azure Event Grid

## Network Solutions

- Considerations

  - Network requirements (naming, regions, subscriptions, IP addresses, segmentation and subnets, filtering)
  - Workload requirements (CIDR ranges, network security groups, network traffic routing)
  - IP range for Virtual Networks /16 or larger
  - Consider hub-spoke network topology

- **Azure Firewall** (works across Azure Virtual Network and subscriptions)

- Network design options

  - **Pattern 1: Single Virtual Network**
    - Single-region only
    - Network Security Groups or Application Security Groups
    - Subnets to e.g. separate workload types
  - **Pattern 2: Multiple Virtual Networks with Peering**
    - Segment based on Virtual Network
    - Peer networks across regions (peering isn't transitive!)
    - Allows for Pattern 1 options as well
  - **Pattern 3: Multiple Virtual Networks in hub-spoke topology**
    - One Virtual Network is the hub in a region
    - Connect spokes to their hub with peering
    - Connect hubs across regions with peering

- **Routing**

  - **Route Tables**
    - **System Routes** (can't be modified, can be overridden with UDRs)
      - Traffic between VM's
      - Virtual network-to-network VPN for VM's
      - Site-to-site through Azure ExpressRoute or Azure VPN Gateway
    - **User-defined Routes**, a.k.a. custom routes
      - Filtering of internet traffic or forced tunneling
      - Flow traffic through NVA
      - Define routes to direct traffic and hops
    - Routes from other Virtual Networks (when peering networks)
    - **Border Gateway Protocol** routes (when on-premises network gateway exchanges BGP routes)
    - Service endpoint routes (when enabling a service endpoint in a subnet)
  - **Azure Virtual Network NAT**
    - Fully managed, highly resilient
    - All outbound connectivity through specified static public IP addresses
    - No load balancer or public IP for VM's needed

- Connect on-premises to Azure

  - **Azure VPN Gateway**
    - Encrypted traffic between Azure Virtual Network and on-premises network
    - Over the public internet
    - Configuration options
      - Site-to-site
      - Point-to-site
      - Virtual network-to-network
  - **Azure ExpressRoute**
    - Private (not over the internet), dedicated connection
    - Requires third-party connectivity provider
    - Can be used with "VPN Failover"
  - **Azure Virtual WAN**
    - Hubs as managed service
    - Virtual Network Connections to Azure
    - Relies on hub-spoke network topology
    - Supports ExpressRoute and VPN Gateway

- **Load balancing** ([see flowchart](./assets/images/az-305-infrastructure-load-balancing-flowchart.png))

  - Aspects
    - Traffic type https? Public or private facing?
    - Global vs regional?
    - Availability and SLA
    - Cost
    - Features and limits
  - Options
    - **Azure Content Delivery Network**
      - For high-bandwidth content
      - Global solution
      - Supports Microsoft, Akamai, Verizon networks
      - Supports custom domains, file compression, caching, geo-filtering
    - **Azure Front Door**
      - Low-latency, prioritization, weighted distribution, affinity, WAF and CDN integration
      - Global (multi-region) failover
    - **Azure Traffic Manager**
      - DNS-based traffic load balancer
      - Distribute traffic across regions
      - High Availability
      - Prioritization, weighted distribution, performance-based, geographic, subnet-based
      - Usually not for HTTPS traffic ([see flowchart](./assets/images/az-305-infrastructure-load-balancing-flowchart.png))
    - **Azure Load Balancer**
      - Layer 4 load-balancing (UDP and TCP)
      - High performance, low-latency
      - Map inbound connections to back-end pool destinations
      - Not global (so may need to be combined with Traffic Manager)
    - **Azure Application Gateway**
      - Layer 7 load-balancing
      - Web traffic load balancer
      - Application Delivery Controller (ADC) as a service
      - Path-based routing and Multiple-site routing

- Application Protection Services
  - **Azure DDoS Protection**
    - Traffic monitoring and DDoS protection
    - Support from DDoS rapid response team
  - **Azure Private Link**
    - Access Azure hosted customer/partner services
    - Works over private endpoint in your own Virtual Network via Azure Backbone
    - Prevents traffic needing to go over the public internet
  - **Azure Firewall**
    - Managed network security service with High Availability
    - Fully stateful firewall as a service
    - Static public IP address
    - Inbound protection (also for non-HTTP(S) protocols like RDP and SSH)
    - Outbound protection for all ports and protocols
    - Application-level protection for outbound HTTP(S)
  - **Azure Web Application Firewall**
    - Protects against common web exploits (e.g. OWASP top 10)
    - Deploy as part of Application Gateway or Front Door
  - **Azure Virtual Network Security Groups** (NSGs)
    - Works towards subnets and/or network interface cards (NICs)
    - Works with Access Control List (ACL) allow/deny rules
    - Prioritized inbound/outbound rules
  - **Service Endpoints** (in Azure Virtual Networks)
    - Secure Azure service resources to virtual networks
    - Optimal routing from Azure services to virtual networks
  - **Azure Bastion**
    - Managed PaaS to secure RDP and SSH over TLS
    - Monitor and manage remote connections

## Migrations

- **Cloud Adoption Framework** (CAF)

  - Initial phases
    - **Define strategy**
      - Understand motivations
      - Business outcomes and justification
      - Prioritize projects
    - **Plan**
      - Digital estate
      - Organization readiness
      - Skill readiness
      - Cloud adoption plan
    - **Ready**
      - Operating model
      - Landing zones
      - Design area guidance
      - Implementation options
    - **Adopt**
      - Migration of workloads
        - Asses (determine costs, modernization, deployment tools)
        - Deploy (replicate or improve in the cloud)
        - Release (after testing release to users)
      - Modernization
      - Innovation
  - Iterative phases
    - **Secure**
      - Risk insights
      - Business resilience
      - Asset protection
    - **Manage**
      - Business commitments
      - Operations baseline and maturity
    - **Govern**
      - Business risks
      - Policy and compliance
      - Governance maturity

- **Azure Migration Framework**

  - Stage 1: Assess on-premises environment
    - Five Rs of Rationalization
      - **Rehost** a.k.a. "Lift and Shift"
      - **Refactor** a.k.a. "Repackaging" to be cloud-native, usually with PaaS features
      - **Rearchitect** modify and extend app functionality to optimize for the cloud
      - **Rebuild** from scratch, for the cloud with best available tools
      - **Replace** with newer solutions, considering SaaS and FaaS options
    - Migration tools and services
      - **Service Map** (Azure Monitor) to identify dependencies (requires agent on VM's)
      - **Azure Total Cost of Ownership (TCO) Calculator**
      - **Azure Migrate** to perform assessment and migration of VM's, servers, databases, etc. with various tools
      - **Data Migration Assistant (DMA)** for SQL Server migrations
      - **Database Migration Service** for other database types
      - **Azure Cosmos DB Data Migration tool**
      - **Microsoft Cost Management** to monitor and optimize ongoing costs
      - **Azure Advisor** to monitor resources for reliability, performance, cost, security
      - **Azure Monitor** for telemetry from both on-premises and Azure resources
      - **Microsoft Sentinel** for security analytics
  - Stage 2: Migrate workloads
    - Deploy cloud infrastructure (optionally with migration tools)
    - Migrate workloads (with a pilot first)
      - **Azure Migrate**
        - Unified platform with single portal
        - Various assessment and migration tools
          - Server Migration
          - SQL Server Data Migration Assistant (DMA)
          - Azure Database Migration Service
          - Web app migration assistant
          - Azure Data Box
        - Different migration feature per workload
        - Azure Migrate hub tools
        - Scenarios
          - Windows Server
          - SQL Server
          - Linux
          - Windows apps, Java apps, PHP apps
          - SAP HANA
          - Specialized compute
      - **Azure Resource Mover**
        - Move resources between subscriptions, resource groups, and regions
        - Test a move before committing
        - Use _before_ migration to organize resources
        - Use _after_ migration to optimize organization
      - **Azure Database Migration Service**
        - Targets
          - VM's with SQL Server
          - Azure SQL Database
          - Azure SQL Managed Instance
          - Azure Cosmos DB
          - Azure Database for MySQL
          - Azure Database for PostgreSQL
        - Modes
          - Online (synchronization and cut over)
          - Offline (with downtime)
        - Steps
          - Step 1: asses databases
          - Step 2: migrate schema in target
          - Step 3: migrate data and verify
        - Requires Virtual Network, NSGs, Azure Windows Firewall, credentials, and target
      - **Azure Storage Migration Service** (from Windows Admin Center)
        - Inventory Windows servers
        - Transfer data
        - Cut over (optionally)
      - **Azure File Sync** (part of Azure Files)
        - Scenarios
          - Replace or supplement on-premises file servers or NAS devices
          - Lift and shift (rehome)
          - Backup and disaster recovery
          - Azure File Sync for performance and distributed caching
      - **Azure Import/Export service**
        - For large quantities of data from on-premises to Azure storage
        - Physical disks shipped to Azure data center
        - Requires BitLocker
        - Requires shipping carrier
        - Scenarios
          - Migration (one-time task)
          - Backup on-premises to Azure Storage
          - Recovery from Azure Storage
          - Distribution to customer sites
      - **Azure Data Box**
        - 80TB device is sent to your location, back to Data Center
        - Includes Data Box service (extension of Azure portal)
        - Shipping handled by Microsoft
        - Scenarios
          - Migration (one time)
          - Initial bulk transfer
          - Periodic uploads
    - Decommission on-premises infrastructure when ready
  - Stage 3: Optimize migrated workloads
    - Analyze and optimize costs based on recommendations
    - Improve workload performance
  - Stage 4: Monitor workloads
    - Capture health and performance
    - Set up alerting and reporting
