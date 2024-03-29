# AZ-305: Data Storage

All notes taken from [the relevant AZ-305 learning path](https://learn.microsoft.com/en-us/training/paths/design-data-storage-solutions/) starting point.

## Data storage for non-relational data

- **Azure Storage Account** (groups all storage services)

  - Considerations
    - Location
    - Compliance
    - Costs
    - Replication scenarios
    - Administrative overhead
    - Data sensitivity
    - Data isolation
  - Redundancy options
    - **Locally Redundant Storage (LRS)**
    - **Zone Redundant Storage (ZRS)**
    - **Paired (secondary) region**
  - Storage Security
    - Security baseline
    - Shared Access Signatures
      Firewall policies and rules
    - Virtual Network Service Endpoints and Private Endpoints
    - Secure transfer
    - Encryption (keys managed by Microsoft or yourself)

- **Azure Blob Storage** (vast amounts, e.g. images and media files)

  - **Tiers**
    - Premium (99.9% SLA, very low-latency, high storage cost, lower transaction cost)
    - Hot (99.9% SLA, 99.99% RA-GRS, low-latency, high storage cost, lower access cost)
    - Cool (99% SLA, 99.9% RA_GRS, low-latency, 30 days minimum storage)
    - Archive (offline tier, latency is hours, 180 days minimum storage, low storage cost)
  - Offers **WORM** (Write Once, Read Many) immutable storage
    - Time-based retention - Hot, Cool, Archive
    - Legal hold - Premium

- **Azure Files** (managed file shares)

  - Encrypted in transit and at rest
  - Supports SMB and NFS protocols
  - Mount shares as-is
  - Mount via Azure File Sync
    - Direct mount
    - Cache files on-premises
  - Tiers
    - Premium (SSDs, high performance, low latency)
    - Transaction optimized (heavy workloads)
    - Hot access (HDDs, general purpose)
    - Cool access (cost-efficient)
  - **Azure NetApp Files** alternative
    - Fully managed
    - Highly available
    - Very high performances and IOPS
    - Enterprise-grade NAS service

- **Azure Managed Disks** (virtualized physical disks)

  - Disk types
    - Ultra-disk: SSD, 2000 Mbps / 160K IOPS
    - Premium SSD: SSD, 900 Mbps / 20K IOPS
    - Standard SSD: SSD, 750 Mbps / 6K IOPS
    - Standard HDD: HDD, 500 Mbps / 2K IOPS
  - Encryption types
    - Azure Disk Encryption (ADE): only VM has access
    - Server-Side Encryption (SSE): encryption at rest in the data center
    - Encryption at host: server hosting the VM provides encryption

- **Azure Queue Storage** (large numbers of messages)

## Data storage for relational data

- **SQL Variants**

  - **Azure SQL Database**
    - Pricing options
      - **Database Transaction Unit (DTU)** model
        - Simple, pre-configured
        - Bundled measure of compute, storage, and IO
        - Not available for Managed Instances
      - **vCore** model
        - Independent scalability of compute, storage, and IO
        - Flexible and transparent
        - Recommended option
      - **Serverless**
        - Compute tier for single databases
        - Bills only for amount of compute used
      - NOTE: **Elastic Pools** allow combining pricing of multiple databases
  - **Managed Instance**
    - Good for "Lift & Shift"
    - PaaS, always uses vCores mode
    - All databases share the same vCores
    - Supports SQL Server Agent
    - Supports Common language runtime (CLR)
    - Supports Database Mail
    - Supports Distributed transactions
    - Supports Machine Learning Services
    - Supports Service Broker
    - Supports Linked servers
    - **SQL Server on Azure Virtual Machines**
      - Full SQL Server capabilities
      - You're fully responsible for OS and SQL Server

- **Scaling options**

  - Vertical Scaling
    - Elastic Pools
    - DTU model requires Basic, Standard, or Premium tier
    - vCore model requires General Purpose or Business Critical tier
  - Horizontal Scaling
    - Sharding: partition data across multiple SQL Databases
    - Read scale-out: partition workload across SQL Databases
    - Availability depends on database type and pricing tier

- **Availability options**

  - General Purpose (or Standard)
    - Gateway
    - Remote storage (except tempdb for primary replica)
    - Data and log files in Azure Premium Storage (LRS), multiple copies
    - Backup files in Azure Standard Storage (RA-GRS)
    - Failover similar to FCI
  - Business Critical (or Premium)
    - Equivalent to Always On Availability Group
    - Data and log files all on local SSD (for latency improvements)
    - Three secondary replicas
  - Hyperscale
    - Only for Azure SL Database
    - Tiered caches and page servers
    - Up to 100 TB databases
    - Snapshots for near-instantaneous backups
    - Restores take minutes (instead of hours or days)

- **Security of Data**

  - **Data at rest: Transparent data encryption (TDE)**
    - Enabled for all new instances
    - Encryption at page level
    - Encryption and decryption when writing and reading from disk
    - Database Encryption Key (DEK)
      - Service-managed (built-in server certificate)
      - Customer-managed
  - **Data in motion: SSL/TLS**
    - optionally add VPN if communicating with on-premises
  - **Data in process: Dynamic data masking**
    - Policy based
    - Masks result sets of queries
    - Configured through portal, PowerShell, or REST API

- **Azure SQL Edge**

  - Containerized Linux application, small startup memory footprint (<500MB)
  - Supports data streams in real time
  - Connected deployment with Azure IoT Edge
  - Disconnected deployment as standalone container or in a cluster

- **Azure Cosmos DB Table Storage**

  - Single-digit millisecond response times
  - Migration from Azure Table Storage is easy (recommended)

## Data integration

- **Azure Data Factory**

  - ETL data integration process
  - Pipelines of ingested data into a single data store
  - Datasets from within your data stores (90+ connectors)
  - Linked services for external data resources
  - Steps
    - Step 1: Connect and collect. Ingest data.
    - Step 2: Transform and enrich. E.g. with Databricks or HDInsight Hadoop.
    - Step 3: CI/CD and publishing.
    - Step 4: Monitor.
  - Integration runtimes
    - Azure
    - Self-hosted
    - Azure-SSIS

- **Azure Data Lake**

  - Ingest data
    - Unplanned (AzCopy, AzCLI, PowerShell, Storage Explorer)
    - Relational (Azure Data Factory)
    - Streaming (Apache Storm, Azure HDInsight, Azure Stream Analytics, etc.)
  - Access stored data
    - GUI, or Azure CLI, HDFS, CLI, or an SDK
  - Configure access control
    - Role-based Access Control (RBAC) or UNIX Access Control Lists (ACLs)

- **Azure Databricks**

  - Managed **Apache Spark** (big data processing)
    - Control Plane (jobs, notebooks, cluster manager, web application, access control, user sessions)
    - Data Plane (data processing and storage)
  - Environments
    - Databricks SQL
    - Databricks Data Science & Engineering
    - Databricks Machine Learning
  - Integration with PowerBI
  - Machine Learning support

- **Azure Synapse Analytics**

  - Massively Parallel Processing (MPP) architecture
  - Control node to interact with applications
  - Compute nodes to run queries
  - **PolyBase** to retrieve and query relational and non-relational data
  - Components
    - SQL pool: serverless and dedicated resource models for node architecture
    - Spark pool: cluster of Apache Spark nodes for processing
    - Pipelines; capabilities of Azure Data Factory
    - Link: connect to Cosmos DB
    - Studio: web-based IDE for Synapse Analytics
  - Scenarios when Synapse is useful
    - Large variety of data sources and/or code-free ETL
    - Machine Learning via Apache Spark
    - Data Lake integration
    - Real-time analytics

- **Data paths**

  - **Warm** data path
    - Near-real-time
    - Example: Azure Stream Analytics
    - Supported via e.g. Azure SQL Database or Azure Cosmos DB
    - Scenario: IoT device data aggregation and insights
  - **Cold** data path
    - Discover patterns over time
    - Support pivots and aggregations
    - Batch later to pre-calculate aggregate views
    - Supported via Azure Storage (Blobs, Data Lake, Files, Ques, Tables)
    - Ideal for keeping original messages
    - Scenario: build machine learning models over time
  - **Hot** data path
    - Real-time processing and display of data
    - Ideal for latency-sensitive data
    - Scenario: real-time alerts for users

- **Azure Stream Analytics**

  - Fully managed PaaS offering
  - Low cost based on Streaming Units
  - Concepts
    - Data streams: continuous data generated by apps, devices, sensors.
    - Event processing: consume and analyze data streams
  - Data formats
    - CSV
    - JSON
    - Avro
  - Ingest from
    - Azure Event Hubs (optionally with Kafka)
    - Azure IoT Hub
    - Azure BLob Storage
  - Analyze
    - Query (SQL-like)
    - Real-time scoring
  - Deliver
    - Alerts and actions
    - PowerBI
    - Azure Synapse
    - Storage or archive
