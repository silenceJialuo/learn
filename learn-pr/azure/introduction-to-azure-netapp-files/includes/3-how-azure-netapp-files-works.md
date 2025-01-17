Here, we discuss how Azure NetApp Files works behind the scenes and how different elements work together to provide a high-performance cloud NAS service. This knowledge helps you evaluate whether Azure NetApp Files is a good solution for migrating your organization's file-based workloads to the Azure cloud.

## Storage hierarchy

One of the most important components of Azure NetApp Files is the storage hierarchy, which determines how much storage is allocated to your workloads and the maximum available throughput. Understanding the storage hierarchy helps you set up and manage Azure NetApp Files.

Watch this video to understand the relationship between the Azure subscription, NetApp accounts, capacity pools, and volumes.

> [!VIDEO https://learn-video.azurefd.net/vod/player?id=e9e9e134-33f3-4a83-9d3d-d1fa35d357cd]

## Service levels

Azure NetApp Files volume performance scales with the size of the volume and the service level. Azure NetApp Files offers three service levels for the capacity pools you create:

- **Standard**: Provides up to 16 MiB/s of throughput and 1000 IOPS per 1 TiB of capacity provisioned. An Azure NetApp Files volume can generate approximately 319.000 IOPS with only 1.5 ms of latency for adjacent virtual machines. Use Standard for static web content, file shares, and database backups.
- **Premium**: Provides up to 64 MiB/s of throughput and 4,000 IOPS per 1 TiB of capacity provisioned. It can generate a maximum of 450,000 IOPS per volume. Premium is comparable to mainstream SSD performance and is suitable for SAP HANA, databases, enterprise apps, virtual desktop infrastructure (VDI), analytics, technical applications, messaging queues, and big data analytics
- **Ultra**: Provides up to 128 MiB/s of throughput and 8,000 IOPS per 1 TiB of capacity provisioned. It can generate a maximum of  450,000 IOPS per volume. Use Ultra for the most performance-intensive applications, such as HPC applications.

Azure NetApp Files also offers storage with **cool access**. With cool access, you experience the same throughput for data in the hot tier, however the throughput may differ for data residing in the cool tier. 

## Quality of service (QoS)

Azure NetApp Files defines two types of quality of service (QoS) for capacity pools:

- **Auto**: Azure NetApp Files automatically assigns a total throughput for each volume based on the service tier and the volume capacity. For example, a Standard tier 2-TiB volume is automatically assigned a maximum throughput of 32 MiB/s (16 MiB/s x 2).

:::image type="content" source="../media/3-auto-qos-chart.png" alt-text="Diagram depicting auto QoS provisioning.":::

- **Manual**: You assign the throughput you need for a volume. For example, a Standard tier 8-TiB capacity pool has a total available throughput of 128 MiB/s (16 MiB/s x 8). For a 2-TiB volume within that capacity pool, you can assign a throughput of around 64 MiB/s, assuming that much throughput budget is still available after provisioning the capacity pool's other volumes.

:::image type="content" source="../media/3-manual-qos-chart.png" alt-text="Diagram depicting manual QoS provisioning.":::

## Connectivity to Azure NetApp Files

A key consideration when evaluating whether to migrate on-premises workloads to Azure NetApp Files is how your existing applications, services, and users connect to the data in its new location.

### Azure virtual networks

Before provisioning an Azure NetApp Files volume, you need to create an Azure virtual network (VNet) or use one that already exists in the same subscription. The VNet defines the network boundary of the volume.

### Subnets

When you create an Azure NetApp Files volume, you assign the volume to a delegated subnet. A delegated subnet is a subnet that's configured with permissions to create resources that are specific to a service, which in this case is Azure NetApp Files. How network nodes connect to Azure NetApp Files in that subnet depends on where those nodes are located. Azure NetApp Files volumes are securely accessible only within a customer’s VNet context. Azure NetApp Files does not provide a publicly (that is, internet) accessible endpoint.

:::image type="content" source="../media/3-how-azure-netapp-files-works-topology.png" alt-text="Diagram depicting the three types of network connectivity supported by Azure NetApp Files." lightbox="../media/3-how-azure-netapp-files-works-topology.png":::

When planning subnets, there are three main scenarios to consider:

- **Connectivity in the same virtual network.** Any resource running on an Azure virtual machine in the same virtual network that contains the delegated subnet can connect to the file storage provided by Azure NetApp Files. In the diagram that follows this list, both VM 3 and Azure NetApp Files Volume 1 reside in the Hub virtual network, so VM 3 has direct access to Volume 1.

- **Connectivity in a peered virtual network.** Any resource running on an Azure virtual machine in a virtual network that is peered to the network containing the delegated subnet can connect to the file storage provided by Azure NetApp Files. In the diagram that follows this list:

    - The Spoke 1 virtual network is peered to the Hub virtual network, so VM 4 has peered access to Azure NetApp Files Volume 1.
    - The Spoke 2 virtual network is peered to the Hub virtual network, so VM 5 has peered access to Azure NetApp Files Volume 1.
    - The Spoke 1 and Spoke 2 virtual networks aren't peered to each other, so VM 4 can't access Azure NetApp Files Volume 3 and VM 5 can't access Azure NetApp Files Volume 2.

- **Connectivity in a hybrid network.** Any resource running in an on-premises network connected to the Azure virtual network that contains the delegated subnet via VPN or ExpressRoute can connect to the file storage provided by Azure NetApp Files. In the following diagram, the on-premises network is connected to the Azure Hub virtual network via a VPN gateway. Enabling the following scenarios: 

    - A resource in the on-premises network has gateway access to any Azure NetApp Files volume in the gateway's virtual network. For example, VM 2 in the on-premises network can connect to Azure NetApp Files Volume 1. 

    - A resource in the on-premises network has gateway access to any Azure NetApp Files volume in a peered virtual network. For example, VM 1 in the on-premises network can connect to Azure NetApp Files Volume 2 (and VM 2 can connect to Azure NetApp Files Volume 3).