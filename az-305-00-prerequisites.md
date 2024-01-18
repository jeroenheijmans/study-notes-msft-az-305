# AZ-305: Prerequisites

All notes taken from [the relevant AZ-305 learning path](https://learn.microsoft.com/en-us/training/paths/microsoft-azure-architect-design-prerequisites/) starting point.

## Core Architectural Components

- Azure Physical Infrastructure

  - Datacenter
  - Region containing 1 or more Datacenters
  - Availability zones: physically separate Datacenters within a Region
  - Region Pairs: 300+ miles apart
  - Sovereign Regions: for US Government and China

- Azure Account
  - Account
    - Requires at least 1 subscription
  - Subscriptions
    - Unit of management, billing, and scale
    - Boundaries
      - Billing boundary
      - Access control boundary
    - Typical usages
      - DTAP
      - Organizational structure
      - Billing purposes
  - Resource Groups
    - Is purely for grouping resources
  - Resources
    - Always belong to 1 Resource Group
  - Management group
    - Hierarchy to apply policies for governance
    - User access to multiple subscriptions

## Azure Compute and Networking

- Virtual Machines
  - Virtual Machine Scale Set: group of identical, load-balanced VM's
  - Virtual Machine Availability Set: staggered updates and varied power/network connectivity
    - Update domain (VM's to be rebooted at the same time)
    - Fault domain (VM's will share power source and network switch)

## Azure Storage Services

TODO

## Azure Identity, Access, and Security

TODO

## Microsoft Cloud Adoption Framework for Azure

TODO

## Microsoft Azure Well-Architected Framework

TODO
