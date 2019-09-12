---
sectionid: vmware
sectionclass: h2
title: VMWare Overview
parent-id: intro
---

## About VMware

[VMware](http://www.vmware.com) is a publicly-traded company which is majority-owned by Dell/EMC. It creates datacenter software for virtualization management, 
including the [vSphere management suite]().

## Understand Azure vs VMware Resourcing

VMware is known for its heavy leveraging of hardware resources, leading customers 
to realize higher rates of utilization for datacenter capital assets.

> By contrast, Azure does not oversubscribe (or overcommit) fabric resources. vmWare admins can sometimes perceive this as a waste of host resources.

Unlike on-prem environments Azure is required to prioritize
consistent performance for all VMs. It must also deliver multi-tenant
security that isn't always a priority for on-prem environments. This means:

-   In most SKU families, CPU cores are committed to a single VM. Dv3, Ev3 and other newer families hyperthreading to better balance
    system utilization of new memory-dense hardware.
      
-   Every page of customer-allocated memory is committed to a single VM.

-   Every page of disk storage is committed to a single disk, and
    every disk is committed to a single VM. Azure does aggregate disk resources at the fabric level.

### Sizing Workloads

Many migrations to Azure will originate with vmWare virtual machines. Azure Migrate 
has very advanced capabilities to deal with performance monitoring and workload sizing 
so that Azure VMs provide appropriate performance for workloads migrated from vmWare.

Azure Migrate optionally compiles performance history for a VM,
measuring CPU and memory utilization, disk (IOPS and throughput) and
network throughput.

> In many cases, Azure Migrate will downsize VM sizing recommendations during assessment. This is often because of on-premises VM
overprovisioning.

This section provides a little background on why on-prem VMs tend to be overprovisioned.

## Understanding Oversubscription and Overprovisioning in VMware

VMware 'oversubscribes' resources to attempt to maximize consolidation and allow its customers to maximize ROI on datacenter
capital expenditure…i.e., more VMs per host. 

### How Oversubscription Works

VMs share runtime on a virtualization host. When a VM is scheduled, it has access to
processor cores and work can be done. When it is de-scheduled, it is unable to use any resources.

> Hypervisors like VMware ESXi can play a 'shell game' to overcommit resources based on actual system load, focused 
on delivering resources to workloads that are busy. However, overhead associated with resource management can 
impact VM performance...sometimes in an unpredictable way.

### Overprovisioning

It is not uncommon for admins to "overprovision" a workload in order
to tell the hypervisor to reserve additional resources. In practice,
overprovisioning can be a way to defend sensitive workloads against
being impacted by hypervisor resource management.

> When overprovisioned workloads are sent to Azure, it can result in
extravagant levels of waste. Special care must be taken with vmWare VMs
to make sure they are sized properly after being moved to Azure.
(Azure Migrate has performan= ce profiling capabilities to help assure
that VMs are sized appropriately after migration).

## Oversubscription Techniques

vmWare provides an array of features that encourage VM oversubscription.

### Memory ([info](https://www.vmware.com/techpapers/2011/understanding-memory-ma))

-   Transparent page sharing (TPS) eliminates duplication of read-only
    pages (i.e., code pages) across VMs on a host
-   Memory 'ballooning' is a process that allows a virtualization host 
    to scavenge low-demand in-memory pages from a
    VM. (This feature is sometimes disabled in production
    environments.) It works by creating artificial memory pressure in a
    VM (inflating the balloon), which causes the guest OS to page
    memory pages to disk. The memory committed to the balloon driver is
    returned to the host pool.
-   Memory page compression - allows memory pages which would
    otherwise be swapped out by the hypervisor to be compressed in
    memory instead.
-   Disk swapping is also possible on extremely memory constrained
    hosts. It is the hypervisor's last resort to make memory available
    in high memory contention situations.
      

### Storage

-   VMware provides 'thin' disk provisioning for direct-attached storage. Thin disks are typically copy-on-write (COW) and only 
    map pages to storage as new blocks are written.
-   Most disk provisioning, thin or otherwise, happens in SAN or network attached storage. 
    > The underlying storage provider is immaterial to the migration process as long as the guest OS sees a formatted disk.
-   Full disk images are created on Azure when migrating. There is no 'thin' provisioning on Azure disk.
      
### CPU

-   VMs are scheduled via time-slicing and cores can be heavily oversubscribed. Many
    organizations set a policy of 2:1, 4:1 or some other factor.
-   Hyperthreads are 'hardware' threads that allow additional
    execution contexts and can opportunistically borrow resources from
    another (stalled) thread on a shared core.
-   Memory availbility and I/O bandwidth (and not CPU) are typically the governing
    factors limiting performance for datacenter workloads.

## Understanding the on-prem environment

### Roles and Responsibilities

Mature datacenter vmWare vSphere environments typically have strict
[separation of
duties](3D"https://docs.vmware.com/en/VMware-Validated-Design/services/in=):

### Infrastructure/operations, which may be further subdivided into:
-   Server hardware management
-   Networking
-   Storage management
-   Security

### VM operations/provisioning - these are the people who:
-   Perform baseline monitoring
-   Provide escalation support for 'server down' calls
-   Boot/reboot/start/stop VMs

### Workload/OS management
-   Monitor OS and basic app issues
-   Create deployments
-   Manage OS deployment templates
-   Deal with clusters and some BC/DR
-   Maintain system updates and report server patch compliance
-   Manage backups

### Applications/DevOps
-   Monitor applications and investigate application issues
-   Enforce applications security standards
-   Deal with applications infrastructure, i.e., web servers, app server clusters, containers, etc
-   Strict role-based access control may mean that admins have very narrowly gated access to the datacenter environment.
-   In heavily regulated/hi= gh governance environments, expect strong configuration controls to delay certain types of requests involving access (i.e., deployment of Azure Migration assessment tools).
-   If the vmWare environment is run by network operations (not atypical), network architecture and security discussions can take extra time.
