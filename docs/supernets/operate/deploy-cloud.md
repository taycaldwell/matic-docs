---
id: supernets-setup-dev-env
title: Deploy a Polygon Supernet Devnet to the Cloud
sidebar_label: Deploy a Supernet Devnet to the Cloud
description: "Learn how to deploy a test Supernet instance to the cloud."
keywords:
  - docs
  - Polygon
  - edge
  - supernets
  - childchain
  - network
  - modular
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

This document provides references to tutorials for setting up and configuring a Supernet devnet on various cloud platforms.

:::warning Breaking changes
Supernets are rapidly evolving towards a production-ready state, and, as a result, instructions and concepts in these guides are subject to change.

Test releases include breaking changes and offer no guarantees, including backward compatibility. Use the current test releases for testing and familiarization only.

It is highly recommended that you do not attempt deployments on your own; for support, please reach out to the Supernets team.
:::

:::caution No more non-bridge mode deployment

The latest v0.9 test release of Supernets no longer makes a distinction between "bridge mode" and "non-bridge mode". The bridge is now included by default and must be used when running Supernets.

The latest release aligns with the original vision of creating "Super networks" that achieves network scalability and sovereignty with an associated rootchain.

If you require a standalone Supernet instance, custom configuration is necessary.

:::

:::caution Not backwards compatible with v0.8

Supernets v0.9 is not backward compatible at this time. You must either start a new blockchain from scratch or wait for future migration scripts to support migration from previous versions.

:::

## Prerequisites

Before diving into any of the tutorials, make sure your environment meets the necessary prerequisites. They can be found **[<ins>here</ins>](/docs/supernets/operate/system.md)**.

:::caution Don't use the develop branch for deployments

Please ensure that you are not running on the `develop` branch, which is the active development branch and include changes that are still being tested and not compatible with the current process.

Instead, use the [<ins>latest release</ins>](/docs/supernets/operate/install.md) for deployments.

:::

<!-- ===================================================================================================================== -->
<!-- ===================================================================================================================== -->
<!-- ===================================================== GUIDE TABS ==================================================== -->
<!-- ===================================================================================================================== -->
<!-- ===================================================================================================================== -->

<Tabs
defaultValue="cloud"
values={[
{ label: 'Cloud deployment', value: 'cloud', },
{ label: 'Tips & Troubleshoot (coming soon)', value: 'troubleshoot', },
]
}>

<!-- ===================================================================================================================== -->
<!-- ==================================================== AWS GUIDE ====================================================== -->
<!-- ===================================================================================================================== -->

<TabItem value="cloud">

| Platform | Guide |
| --- | --- |
| Amazon Web Services | To set up a devnet on AWS, you can refer to the AWS deployment guide available [<ins>here</ins>](https://github.com/maticnetwork/terraform-polygon-supernets). The guide provides comprehensive instructions on how to use Terraform to set up a Virtual Private Cloud (VPC), subnets, security groups, and EC2 instances, followed by instructions on configuring nodes using Ansible. |
| Microsoft Azure | To set up a devnet on Azure, you can refer to the Azure deployment guide available [<ins>here</ins>](https://github.com/caleteeter/polygon-azure). This repository offers an Azure template that can be deployed through the Azure and Bicep CLI, or directly through the "Deploy to Azure" button. Additionally, the deployment can be viewed via the "Visualize" button available in the repository. |
| Google Cloud Platform | Coming soon. |

</TabItem>
<TabItem value="troubleshoot">

:::info Coming soon
:::

</TabItem>
</Tabs>
