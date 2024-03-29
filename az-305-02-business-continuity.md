# AZ-305: Business Continuity

All notes taken from [the relevant AZ-305 learning path](https://learn.microsoft.com/en-us/training/paths/design-business-continuity-solutions/) starting point.

## High Availability and Disaster Recovery (HADR)

- Recovery characteristics

  - **Recovery Time Objective (RTO)**: maximum time available to recover
  - **Recovery Point Objective (RPO)**: maximum data loss after recovery

- **IaaS HADR**

  - **SQL Server HADR**
    - **Always on Failover Cluster Instance (FCI)**
      - protects instance
      - also captures things outside the database
      - Unique-name and load balancer based
      - One database copy on shared storage (e.g. Azure Premium File Share)
      - Operating System HADR for SQL Server deployments
        - Windows Server Failover Cluster (WSFC)
        - Linux Pacemaker
    - **Always On Availability Group (AG)**
      - protects database
      - does not capture things outside the database
      - primary read/write replica
      - secondary replica's synchronized by transporting logs
      - Standard Edition has 1 primary + 1 secondary replica
      - Enterprise Edition has 1 primary + 8 secondary (max)
    - **Log Shipping**
      - protects database
      - oldest HADR method
      - full backup, restore on secondary, which receives log backups
  - **Virtual Machine HADR**
    - **Availability Sets**
      - Requires Internal Load Balancer (ILB)
      - Guarantees running on different physical servers
      - Fault Domain separation (hardware failures, network outages, power interruptions)
      - Update Domain separation (groups that can be rebooted together)
    - **Availability Zones**
      - Requires Internal Load Balancer (ILB)
      - Guarantees running in different data centers (same region)
      - Mutually exclusive with Availability Sets
      - Typically 1ms latency
    - **Azure Site Recovery**
      - Guarantees replica in different region
      - Monthly RTO of 2 hours
      - No transaction guarantees for VM's running SQL Server

- **PaaS HADR**

  - SQL Server
    - Active geo-replication (Azure SQL Database)
    - Auto-failover Groups (Azure SQL Database or Azure SQL Database Managed Instance)
  - Azure Database for MySQL
    - SLA: 99.99 availability
    - Node-level (e.g. hardware issues): built-in failover mechanism
  - Azure Database for PostgreSQL
    - Similar to MySQL
    - Scale-out hyperscale "Citus" is another option (creates replicas, so more expensive)

- **Hybrid HADR**

  - Transactional Replication from on-premises to Azure SQL Managed Instance (but not other way around)
  - Availability Group between on premises and in Azure
    - needs ExpressRoute or site-to-site VPN
    - needs AD DS and DNS set up

## Backup and Disaster Recovery

- Considerations

  - What are your workloads? (distinct, logically separate capability or task)
  - What are usage patterns? (e.g. prime time)
  - What are the availability metrics?
    - Mean Time to Recovery (MTTR)
    - Mean Time Between Failures (MTBF)
  - What are the recovery metrics?
    - Recovery Time Objective (RTO), max time to recovery
    - Recovery Point Objective (RPO), max data loss
    - Recovery Level Objective (RLO), granularity of recovery
  - What workload availability targets?
    - SLA's for each workload
  - What are the SLAs?

- **Azure Backup**

  - Backup Types
    - On-premises
      - **Microsoft Azure Recovery Services (MARS) agent**: files, folders, system state
      - **System Center Data Protection Manager (DPM)**
      - **Microsoft Azure Backup Server (MABS) agent**: Hyper-V and VMware virtual machines
    - Azure Virtual Machines
      - Entire Machines (Windows or Linux)
      - Files, folders, and system state (with MARS agent)
    - Azure Files (to a storage account)
    - SQL Server in Azure VM's
    - SAP HANA databases in Azure VM's
    - Microsoft cloud (for on-premises or off-site backup)
  - Backup Storage
    - Vaults
      - Azure Backup vault (Azure Backup)
      - Azure Recovery Services vault (Azure Backup or Azure Site Recovery)
    - Redundancy
      - Locally Redundant Storage (LRS), for data center failures
      - Geo-redundant Storage (GRS), for region-wide outages
  - Blob backup and recovery
    - Operational Backup
      - Continuous backup
      - Specific period for restoration
      - Soft delete feature (for blobs and containers) - 1-365 days
      - Blob versioning support for automatic versions
  - Azure Files
    - Share Snapshots
      - Manual creation (portal, REST API, SDK's, Azure CLI, Powershell)
      - Automated creation (Azure Backup (recommended), backup policies)
      - Created at root, retrieval at individual file level
      - Incremental between snapshots
      - Prevents deletion of a share itself (until snapshots are deleted first)
      - Allows for
        - Instant restore
        - Alerts and reporting
        - Self-service restore (via Volume Shadow Copy Service, VSS)
        - On-demand backups can add to scheduled backups
  - Virtual Machine Backup and Recovery
    - **Azure Backup**
      - Specialized offering for database workloads (e.g. SQL Server)
      - Step 1 = VM Snapshot, Step 2 = snapshot into recovery services vault
      - Encrypted at rest with Storage Service Encryption (SSE)
      - Recommend at least "v2 storage" account to prevent throttling
      - Cross Region Restore (CRR) is opt-in for any Recovery Services vault.
  - **Azure SQL Backup and Recovery**
    - SQL Database and SQL Managed Instance
      - Full backups (every week)
      - Differential Backups (every 12-24 hours)
      - Transactional backups (every 5 to 10 minutes)
    - Restore options
      - Restore to Point-in-Time
      - Restore deleted database to time of deletion
      - Restore database to another geographic region
      - Restore database from long-term backup, if Long-Term Retention (LTR) is set up

- **Azure Site Recovery**

  - Features
    - Replicate Azure VM's, also to a secondary region continuously
    - Replicate on-premises VM's
    - Replicate workloads
    - Automate Business Continuity and Disaster Recovery (BCDR) tasks
