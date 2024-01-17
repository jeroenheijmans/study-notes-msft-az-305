# AZ-305: Data Storage

All notes taken from [the relevant AZ-305 learning path](https://learn.microsoft.com/en-us/training/paths/design-data-storage-solutions/) starting point.

- **Data storage for non-relational data**

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
      - Locally Redundant Storage (LRS)
      - Zone Redundant Storage (ZRS)
      - Paired (secondary) region
    - Storage Security
      - Security baseline
      - Shared Access Signatures
        Firewall policies and rules
      - Virtual Network Service Endpoints and Private Endpoints
      - Secure transfer
      - Encryption (keys managed by Microsoft or yourself)
  - **Azure Blob Storage** (vast amounts, e.g. images and media files)
    - Tiers
      - Premium (99.9% SLA, very low-latency, high storage cost, lower transaction cost)
      - Hot (99.9% SLA, 99.99% RA-GRS, low-latency, high storage cost, lower access cost)
      - Cool (99% SLA, 99.9% RA_GRS, low-latency, 30 days minimum storage)
      - Archive (offline tier, latency is hours, 180 days minimum storage, low storage cost)
    - Offers WORM (Write Once, Read Many) immutable storage
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

- **Data storage for relational data**

  - TODO

- **Data integration**
  - TODO
