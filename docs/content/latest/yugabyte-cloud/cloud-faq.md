---
title: Yugabyte Cloud FAQ
linkTitle: Yugabyte Cloud FAQ
description: Yugabyte Cloud frequently asked questions.
headcontent:
image: /images/section_icons/index/quick_start.png
menu:
  latest:
    identifier: cloud-faq
    parent: yugabyte-cloud
    weight: 900
isTocNested: true
showAsideToc: true
---

## Yugabyte Cloud

### What is Yugabyte Cloud?

Yugabyte Cloud is a fully managed YugabyteDB-as-a-Service that allows you to run YugabyteDB clusters on Google Cloud Platform (GCP), Amazon Web Services (AWS), and Microsoft Azure (coming soon).

You access your Yugabyte Cloud clusters via [YSQL](../../api/ysql) and [YCQL](../../api/ycql) client APIs, and administer your clusters using the Yugabyte Cloud console.

See also [Yugabyte Cloud](https://www.yugabyte.com/cloud/) at yugabyte.com.

Yugabyte Cloud runs on top of [Yugabyte Platform](../../yugabyte-platform/).

### How is Yugabyte Cloud priced?

Yugabyte bills for its services as follows:

- Charges by the minute for your Yugabyte Cloud clusters.
- Tabulates costs daily.
- Displays your current monthly costs under **Invoices** on the **Billing** tab.

For information on Yugabyte Cloud pricing, refer to the [Yugabyte Cloud Standard Price List](https://www.yugabyte.com/yugabyte-cloud-standard-price-list/). For a description of how cluster configurations are costed, refer to [Cluster costs](../cloud-admin/cloud-billing-costs).

<!--
### Is support included in the base price?

### What SLAs are guaranteed for support and service?
-->

### What are the differences between Free and Paid clusters?

Use the **Free** cluster to get started with YugabyteDB. The free cluster is limited to a single node, and although not suitable for production workloads, the cluster includes enough resources to start exploring the core features available for developing applications with YugabyteDB. You can only have one Free cluster.

**Paid** clusters can have unlimited nodes and storage and are suitable for production workloads. Paid clusters support horizontal and vertical scaling - nodes and storage can be added or removed to suit your production loads. Paid clusters also support VPC peering, and scheduled and manual backups.

A Yugabyte Cloud account is limited to a single Free cluster; you can add as many Paid clusters as you need.

| Feature | Free | Paid |
| :----------- | :---------- | :---------- |
| Cluster | Single Node | Any |
| vCPU/Storage | Up to 2 vCPU / 10 GB RAM | Any |
| Regions | All | All |
| Upgrades | Automatic | Automatic |
| VPC Peering | No | Yes |
| Fault Tolerance | None (Single node, RF -1) | Multi node RF-3 clusters with Availability zone and Node level |
| Scaling | None | Horizontal and Vertical |
| Backups | None | Scheduled and on-demand |
| Support | Slack Community | Enterprise Support |

### Can I migrate my Free cluster to a Paid cluster?

Currently self-service migration is not supported. Contact [Yugabyte Support](https://support.yugabyte.com/hc/en-us/requests/new?ticket_form_id=360003113431) for help with migration.

### What regions in AWS and GCP are available?

Yugabyte Cloud supports all the regions that have robust infrastructure and sufficient demand from customers. Regions where load balancers, certain VMs, or other resources are harder to obtain have not yet been added. We are continuously improving region coverage, so if there are any regions you would like us to support, reach out to [Yugabyte Support](https://support.yugabyte.com/hc/en-us/requests/new?ticket_form_id=360003113431).

### How do I connect to my cluster?

You can connect to clusters in the following ways:

Cloud Shell
: Run the [ysqlsh](../../admin/ysqlsh) or [ycqlsh](../../admin/ycqlsh) shell from your browser to connect to and interact with your YugabyteDB database. Cloud shell does not require a CA certificate or any special network access configured.

Client Shell
: Connect to your YugabyteDB cluster using the YugabyteDB [ysqlsh](../../admin/ysqlsh) and [ycqlsh](../../admin/ycqlsh) client shells installed on your computer.

: Before you can connect using a client shell, you need to have an IP allow list or VPC peer set up. Refer to [Assign IP Allow Lists](../cloud-basics/add-connections/).

Applications
: Applications connect to and interact with YugabyteDB using API client libraries, also known as a client drivers. Before you can connect a application, you will need to install the correct driver. Clusters have SSL (encryption in-transit) enabled so make sure your driver details include SSL parameters. For information on available drivers, refer to [Build an application](../../quick-start/build-apps). 

: Before you can connect, your application has to be able to reach your Yugabyte Cloud. To add inbound network access from your application environment to Yugabyte Cloud, add the public IP addresses to the [cluster IP access list](../cloud-basics/add-connections), or use [VPC peering](../cloud-network/vpc-peers) to add private IP addresses.

For more details, refer to [Connect to clusters](../cloud-basics/connect-to-clusters). 

## Security

### How secure is my cluster?

Your data is processed at the Yugabyte Cloud account level, and each cloud account is a single tenant, meaning it runs its components for only one customer. Clusters in your cloud are isolated from each other in a separate VPC, and access is limited to the IP addresses you specify in allow lists assigned to each cluster. Resources are not shared between clusters.

Yugabyte Cloud uses both encryption in transit and encryption at rest to protect clusters and cloud infrastructure, and provides DDoS and application layer protection, and automatically blocks network protocol and volumetric DDoS attacks.

Yugabyte Cloud uses a shared responsibility model for cloud security. For more information on Yugabyte Cloud security, refer to [Cloud security](../cloud-security/).

## Cluster configuration and management

### What cluster configurations can I create?

From the cloud console you can create single region clusters that can be deployed across multiple and single availability zones. 

The Fault Tolerance of a cluster determines how resilient the cluster is to node and cloud zone failues and, by extension, the cluster configuration. You can configure clusters with the following fault tolerances in Yugabyte Cloud:

- **None** - single node, with no replication or resiliency. Recommended for development and testing only.
- **Node Level** - a minimum of 3 nodes deployed in a single availability zone with a [replication factor](../../architecture/docdb-replication/replication/) (RF) of 3. YugabyteDB can continue to do reads and writes even in case of a node failure, but this configuration is not resilient to cloud availability zone outages. For horizontal scaling, you can scale nodes in increments of 1. 
- **Availability Zone Level** - a minimum of 3 nodes spread across multiple availability zones with a RF of 3. YugabyteDB can continue to do reads and writes even in case of a cloud availability zone failure. This configuration provides the maximum protection for a data center failure. Recommended for production deployments. For horizontal scaling, nodes are scaled in increments of 3.

Free clusters are limited to a single node in a single region.

For multi-region deployments, including [synchronous replication](../../explore/multi-region-deployments/synchronous-replication-ysql/), [asynchronous replication](../../explore/multi-region-deployments/asynchronous-replication-ysql/), and [geo-level partitioning](../../explore/multi-region-deployments/row-level-geo-partitioning/), contact [Yugabyte Support](https://support.yugabyte.com/hc/en-us/requests/new?ticket_form_id=360003113431).

### What is the upgrade policy for clusters?

Upgrades are automatically handled by Yugabyte. There are two types of upgrades:

- Cloud console - During a maintenance window, Yugabyte Cloud console may be in read-only mode and not allow any edit changes. The upgrade has no impact on running clusters. Customers will be notified in advance of the maintenance schedule.

- Cluster (yugabyteDB) version upgrade - To keep up with the latest bug fixes, improvements, and security fixes, Yugabyte will upgrade your cluster to the latest version. We will notify customers of any upcoming upgrade schedule via email and Slack. All database upgrades are done on a rolling basis to avoid any downtime. 

<!--
### How do I migrate my database to Yugabyte Cloud
-->

## Backups

### How are clusters backed up?

Currently, Yugabyte Cloud does not support backups of free clusters.

By default, every paid cluster is backed up automatically every 24 hours, and these automatic backups are retained for 8 days. The first automatic backup is triggered within the first 10 minutes of creating a table, and scheduled for every 24 hours from the initial backup. You can change the default policy using your own scheduled backup policy.

Yugabyte Cloud runs full backups, not incremental.

Backups are retained in the same region as the cluster.

Backups for AWS clusters are encrypted using AWS S3’s server-side encryption and backups for GCP clusters are encrypted using Google-managed server-side encryption keys.

### Can I download backups?

Currently, Yugabyte Cloud does not support self-service backup downloads. Contact [Yugabyte Support](https://support.yugabyte.com/hc/en-us/requests/new?ticket_form_id=360003113431) for assistance.
