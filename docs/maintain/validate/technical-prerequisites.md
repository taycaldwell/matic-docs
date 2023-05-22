---
id: technical-prerequisites
title: Technical Prerequisites
description: Technical prerequisites before setting up the validator node along with mandatory checks and best practices after setting up the Validator node.
keywords:
  - docs
  - polygon
  - technical prerequisites
  - validator node
  - ansible
image: https://wiki.polygon.technology/img/polygon-logo.png
---

This document provides a detailed description of the mandatory technical prerequisites before setting up a Validator node on the Polygon network. It also provides useful checks along with best practices to operate Validator nodes.

## Before setting up the Validator node

:::caution

We recommend running the Validator node with Sentry. Otherwise, you may encounter security concerns and issues with your Validator node.

:::

### Downloading the Snapshot

It is recommended that you keep your snapshots handy before setting up the Validator node. Link to download and extract the snapshots: https://snapshots.polygon.technology/

### RPC Endpoint for Node Setup

Validator nodes require an Ethereum-based RPC endpoint. You may use your own Ethereum node or utilize external infrastructure providers in our list. In order to see the list, click on the dropdown menu labeled **Infra Providers** in the navigation bar.

### Open necessary ports

- **Port 26656** &rarr; Heimdall service will connect your node to another node's Heimdall service using this port.

- **Port 30303** &rarr; Bor service will connect your node to another node's Bor service using this port.

- **Port 22** &rarr; for the Validator to be able to SSH from wherever they are.

### Install RabbitMQ

It is recommended to install the **RabbitMQ** service before setting up your Validator node. Please utilize the below commands to set up RabbitMQ (if not already installed):

```bash
sudo apt-get update
sudo apt install build-essential
sudo apt install erlang
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.10.8/rabbitmq-server_3.10.8-1_all.deb
sudo dpkg -i rabbitmq-server_3.10.8-1_all.deb
```

## Mandatory Checklist for Validator Setup

1. Please follow the below checklist in order to set up your Validator node using Binaries, Ansible or Packages.

    | Checklist | Binaries | Ansible | Packages |
    |----------------------------|---------------|--------------|-----------|
    | **Machines Required** | 2 Machines - **Sentry** & **Validator** | 3 Machines - **Local Machine**, **Sentry** and **Validator** | 2 Machines - **Sentry** & **Validator** |
    | **Install Go Packages** | Yes | No | No |
    | **Install Python** | No | Yes (only on the **Local Machine** where the **Ansible Playbook** runs) | No |
    | **Install Ansible** | No | Yes (only on one machine) | No |
    | **Install Bash** | No | No | Yes |
    | **Run Build Essential** | Yes | No | No |
    | **Node Setup** | [Using Binaries](/maintain/validate/run-validator-binaries.md) | [Using Ansible](/maintain/validate/run-validator-ansible.md) | [Using Packages](/maintain/validate/run-validator.md) |

2. Once your Sentry and Validator nodes are synced and running, head over to our [Discord server](https://discord.com/invite/0xPolygon) and ask the community to health-check your nodes. You may check the logs by using the following commands:

    - **Heimdall logs &rarr;** ```journalctl -u heimdalld.service -f```
    - **Bor logs &rarr;** ```journalctl -u bor.service -f```

3. It is highly recommended to maintain a backup of the key files on your local machine. These might be needed in the situation of migration or outage. You may access the files using the commands below:

    ```bash
    vi ~/.heimdalld/config/priv_validator_key.json
    vi ~/.bor/keystore/UTC--XXXXX
    vi ~/.bor/password.txt
    vi /etc/matic/metadata
    ```

4. Ensure that Bor and Heimdall are on their right versions. Commands to verify the versions are provided below:

    ```bash
    heimdalld version
    bor version
    ```

5. It is recommended to always maintain 2 sentries to maximize your node uptime.

6. Constantly keep checking the peer count on Heimdall and Bor service using the following commands:

    ```bash
    # Heimdall
    curl localhost:26657/net_info? | jq .result.n_peers

    # Bor
    bor attach .bor/data/bor.ipcadmin.peers.length
    ```

    :::info

    For Validators, **the output should be only one peer, which has to be a Sentry**. Validators should connect only to the Sentry and not with external peers.

    Sentry can connect with multiple peers.

    :::