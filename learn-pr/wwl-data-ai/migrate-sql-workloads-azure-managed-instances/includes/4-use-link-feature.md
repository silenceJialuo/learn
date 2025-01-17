Azure SQL Managed Instance link feature offers a true online migration option compared to all other services and tools available. In addition, by partially running workloads on Azure, organizations can adopt a phased approach to cloud migration while still taking advantage of the benefits that Azure provides.

While the link is designed to replicate one database per link, it can be configured to replicate multiple databases from a single SQL Server instance to one or more SQL managed instances, or even replicate the same database to multiple SQL managed instances. This is achieved by setting up multiple links, each corresponding to a database-managed instance pair.

### Hybrid flexibility with Azure SQL Managed Instance link feature

Azure SQL Managed Instance link feature allows you to replicate your SQL Server databases hosted anywhere to Azure, and fail over to the cloud in the event of a disaster or major business disruption. Azure SQL Managed Instance link also ensures seamless failover between the primary and secondary databases.

One of the advantages of using Azure SQL Managed Instance is that it's a platform as a service (PaaS), which means that the latest hardware maintenance, patching, and updates are applied and managed automatically by Azure. This ensures that your database environment is always up-to-date and secure, while also reducing the risk of downtime due to hardware failures or software vulnerabilities.

:::image type="content" source="../media/module-2-sql-managed-instance-link-feature.png" alt-text="Diagram showing the replication of databases from the primary replica to the secondary replica through the distributed availability group (AG).":::

As we can see above, the link feature uses distributed availability group (DAG) and it's scoped per database (one link per one database). This allows you to consolidate multiple parallel SQL Server databases into an Azure SQL Managed Instance or scale them out across multiple instances and regions worldwide.

The link feature provides two types of replication:

- **One-way replication**. One-way replication is available for SQL Server versions 2016 and 2019 and allows you to replicate data one way from a SQL Server instance to your managed instance. 

- **Two-way replication**. SQL Server 2022 provides a two-way replication feature, where you can replicate data between your managed instance and SQL Server instances, manually fail over during a disaster, and manually fail back after the disaster is mitigated. It supports an online failover but an offline failback. A preview of the online failback is available to sign up for.

### Extended capabilities to the cloud

In addition to migrate workloads, there are several ways to use the link feature and use Azure services and resources, which include:

| Feature | Description |
| --- | --- |
| **Offload read-only workloads** | You may want to configure secondary replicas on your SQL Server to Azure to offload reporting needs. The link feature is database scoped, allowing for consolidation of read-only workloads in Azure, which can be used to bring data closer to customers in any supported region worldwide with minimum effort. |
| **Automated backups** | Secondary replicas running on Azure SQL Managed Instance are automatically backed up to your Azure Blob Storage account, which significantly reduces administrative efforts and improve reliability. |
| **Business continuity** | As a disaster recovery solution, the link feature allows you to fail over to Azure SQL Managed Instance and fail back after the disaster is mitigated. |

### Enable the link feature

To configure the link feature, you must follow the same steps regardless of whether you're migrating to Azure SQL Managed Instance, configuring disaster recovery on the cloud, offloading workloads to Azure, or aiming to reduce backup operations and management costs.

You can use either a wizard in SQL Server Management Studio (SSMS) or scripts. The main advantage of using scripts is that they can be automated, which can improve your deployment process, saving time and effort. 

- Replicate a database by using [Azure SQL Managed Instance link wizard available in SSMS](/azure/azure-sql/managed-instance/managed-instance-link-use-ssms-to-replicate-database).

- Replicate a database by using [T-SQL and PowerShell scripts.](/azure/azure-sql/managed-instance/managed-instance-link-use-scripts-to-replicate-database)

There are a few SQL Server features that aren't supported by Azure SQL Managed Instance link. For example, you can't enable the link feature if the functionality that's used on the primary  SQL Server database isn't supported on Azure SQL Managed Instance, such as file tables and file streams.

For the full list of supported features, see [Limitations of Azure SQL Managed Instance link](/azure/azure-sql/managed-instance/managed-instance-link-feature-overview?#limitations).

As we've seen, Azure SQL Managed Instance link feature enables organizations to confidently extend their SQL Server environments to Azure while also benefiting from the scalability, performance, and security features that Azure SQL Managed Instance offers.
