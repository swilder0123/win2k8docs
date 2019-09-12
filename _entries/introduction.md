# Introduction

## Purpose of this notebook

This notebook is not for architectural planning. It is to assist as a reference to Windows Server in migration planning, with plenty of links to old articles, datacenter considerations, etc. Hopefully it will save you time and be a useful shortcut to get to information you need.

Architectural and Planning information is available from these resources...these are referenced throughout this notebook: 
- The Azure Cloud Adoption Framework - Provides a foundation for planning a migration target environment.
- Windows Server 2008/SQL Server 2008 Migration Playbooks - 
- Azure Architectural Design documentation 

DO use this notebook to find resources aimed at specific implementation issues which have already come up with various Azure customers.

You can also read through "Reader's Digest" versions of product overviews and guidance. I have put in quite a few links that you can use to reference source material, learn more, etc. 

## What is Azure Migration?

Azure Migration uses Azure Site Recovery replication together with a variety of assessment tools to successfully duplicate server on-prem server configurations in Microsoft Azure. These are typically called "lift-and-shift" migrations.

The [Azure Migration Guide](https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption/migrate/azure-migration-guide/?tabs=Overview) is the principal resource in understanding how Azure Migration works. Specifically it provides a roadmap to important steps for any migration, including assessment, migration, optization and potential later workload transformation.

### SOME OTHER RESOURCES

[Cloud Migration](https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption/migrate/azure-migration-guide/index?tabs=Overview) is a topic within the Cloud Adoption Framework (CAF) that lays out a process for how customers can move existing on-prem assets to Azure. 

Azure Migrate is a service built on Azure Site Recovery technology to 'lift-and-shift' on-prem servers to Azure.

Azure Database Migration Service migrates compatible on-prem databases to Azure managed data services. The Data Migration Guide covers a large set of migration scenarios and provides guidance, partner tools, etc.

Cloud Migration includes ample documentation on preparing for migrations. Expect many customer scenarios based on datacenter standards, deployment practices, etc.



```
   - I am putting some really boring rote text in here for you to ignore.
   
   - here is a trivial update
   
```


### SPECIAL CHALLENGES FOR WINDOWS SERVER 2008 AND 2008 R2 MIGRATION

- 32- vs. 64-bit OS migration - Windows Server 2008 was the final version of Windows Server to be available in a 32-bit distribution.
- Features - Key OS features, like Storage Spaces, Clustering, network virtualization, etc. are either completely missing in Windows Server 2008 or have substantially reduced functionality.
- Management tools connectivity - remote admin tools are version sensitive. Typically downlevel client versions are required for remote management tool connectivity.
- Age of documentation - in some cases, documents may be missing, or links from existing pages may not work.

