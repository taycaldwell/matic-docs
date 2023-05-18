---
id: full-node-deployment
title: Run a Full Node using Ansible
sidebar_label: Using Ansible
description: Deploy a Full Node using Ansible
keywords:
  - docs
  - polygon
  - matic
  - node
  - full node setup
  - ansible
  - deploy
image: https://wiki.polygon.technology/img/polygon-wiki.png
---

import useBaseUrl from '@docusaurus/useBaseUrl';

This tutorial guides you through starting and running a full node using Ansible.

An [Ansible playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html) is used to
configure and manage a full node. See the [Minimum Technical Requirements](technical-requirements.md) guide for the system requirements.

:::tip Snapshots

Steps in this guide involve waiting for the Heimdall and Bor services to sync fully. This process takes several days to complete.

Please use snapshots for faster syncing without having to sync over the network. For detailed instructions, see [<ins>Snapshot Instructions for Heimdall and Bor</ins>](/operate/snapshot-instructions-heimdall-bor.md).

For snapshot download links, see the [<ins>Polygon Chains Snapshots</ins>](https://snapshot.polygon.technology/) page.

:::

## Prerequisites

- Install Ansible on your local machine with Python3.x. The setup will not work if you have Python2.x.
    - To install Ansible with Python 3.x, you can use pip. If you do not have pip on your machine,
      follow the steps outlined [here](https://pip.pypa.io/en/stable/). Run `pip3 install ansible` to install
      Ansible.
- Check the [Polygon PoS Ansible repository](https://github.com/maticnetwork/node-ansible#requirements) for
  requirements.
- You will also need to ensure that Go is **not installed** in your environment. You will run into issues if you attempt to set up your full node through Ansible with Go installed as Ansible requires specific packages of Go to be installed.
- You will also need to make sure that your VM / Machine does not have any previous setups for Polygon Validator or Heimdall or Bor. You will need to delete them as your setup will run into issues.

:::info Heimdall source enhancements

The latest Heimdall version, **[v.0.3.3](https://github.com/maticnetwork/heimdall/releases/tag/v0.3.3)**, contains a few enhancements.
The delay time between the contract events of different validators **has been increased** to ensure that the mempool doesn't get filled
quickly in case of a burst of events that could hamper the chain's progress.

Additionally, the data size **has been restricted in state sync txs to 30Kb (when represented in bytes) and 60Kb (when defined as string)**.
For example:

```bash
Data - "abcd1234"
Length in string format - 8
Hex Byte representation - [171 205 18 52]
Length in byte format - 4
```
:::

## Full node setup

- Ensure you have access to the remote machine or VM on which the full node is being set up.
  > Refer to [https://github.com/maticnetwork/node-ansible#setup](https://github.com/maticnetwork/node-ansible#setup) for more details.
- Clone the [https://github.com/maticnetwork/node-ansible](https://github.com/maticnetwork/node-ansible) repository.
- Navigate into the node-ansible folder: `cd node-ansible`
- Edit the `inventory.yml` file and insert your IP(s) in the `sentry->hosts` section.
  > Refer to [https://github.com/maticnetwork/node-ansible#inventory](https://github.com/maticnetwork/node-ansible#inventory) for more details.
- Check if the remote machine is reachable by running: `ansible sentry -m ping`
- To test if the correct machine is configured, run the following command:

  ```bash
  # Mainnet:
  ansible-playbook playbooks/network.yml --extra-var="bor_version=v0.3.9 heimdall_version=v0.3.3 network=mainnet node_type=sentry" --list-hosts

  # Testnet:
  ansible-playbook playbooks/network.yml --extra-var="bor_version=v0.3.9 heimdall_version=v0.3.3 network=mumbai node_type=sentry" --list-hosts
  ```

  <img src={useBaseUrl("img/network/full-node-mumbai.png")} />

- Next, set up the full node with this command:

  ```bash
  # Mainnet:
  ansible-playbook playbooks/network.yml --extra-var="bor_version=v0.3.9 heimdall_version=v0.3.3 network=mainnet node_type=sentry"

  # Testnet:
  ansible-playbook playbooks/network.yml --extra-var="bor_version=v0.3.9 heimdall_version=v0.3.3 network=mumbai node_type=sentry"
  ```

- In case you run into any issues, delete and clean the whole setup using:
  ```
  ansible-playbook playbooks/clean.yml
  ```

- Once you initiate the Ansible playbook, log in to the remote machine.

- Please **ensure that the value of seeds and bootnodes mentioned below is the same value as mentioned in Heimdall and Bor `config.toml` files**. If not, change the values accordingly.

  - Heimdall seed nodes:

    ```bash
    moniker=<enter unique identifier>

    # Mainnet:
    seeds="2a53a15ffc70ad41b6876ecbe05c50a66af01e20@3.211.248.31:26656,6f829065789e5b156cbbf076f9d133b4d7725847@3.212.183.151:26656,7285a532bad665f051c0aadc31054e2e61ca2b3d@3.93.224.197:26656,0b431127d21c8970f1c353ab212be4f1ba86c3bf@184.73.124.158:26656,f4f605d60b8ffaaf15240564e58a81103510631c@159.203.9.164:26656,31b79cf4a628a4619e8e9ae95b72e4354c5a5d90@44.232.55.71:26656,a385dd467d11c4cdb0be8b51d7bfb0990f49abc3@35.199.4.13:26656,daad548c0a163faae1d8d58425f97207acf923fd@35.230.116.151:26656,81c76e82fcc3dc9a0a1554a3edaa09a632795ea8@35.221.13.28:26656"

    # Testnet:
    seeds="e72c0466a02ea43b2198bd3a9454b87a3ef0d77e@54.147.31.250:26656,b302d1ddb21102e794b524d05152a7834da05bd1@34.226.134.117:26656,9dfc12d9f39257cefc3d57a4d7302586e59a994e@18.213.200.99:26656"
    ```
  - Bootnodes:

    ```bash
    # Mainnet:
    bootnode ["enode://0cb82b395094ee4a2915e9714894627de9ed8498fb881cec6db7c65e8b9a5bd7f2f25cc84e71e89d0947e51c76e85d0847de848c7782b13c0255247a6758178c@44.232.55.71:30303","enode://88116f4295f5a31538ae409e4d44ad40d22e44ee9342869e7d68bdec55b0f83c1530355ce8b41fbec0928a7d75a5745d528450d30aec92066ab6ba1ee351d710@159.203.9.164:30303","enode://4be7248c3a12c5f95d4ef5fff37f7c44ad1072fdb59701b2e5987c5f3846ef448ce7eabc941c5575b13db0fb016552c1fa5cca0dda1a8008cf6d63874c0f3eb7@3.93.224.197:30303","enode://32dd20eaf75513cf84ffc9940972ab17a62e88ea753b0780ea5eca9f40f9254064dacb99508337043d944c2a41b561a17deaad45c53ea0be02663e55e6a302b2@3.212.183.151:30303"]

    # Testnet:
    bootnodes ["enode://320553cda00dfc003f499a3ce9598029f364fbb3ed1222fdc20a94d97dcc4d8ba0cd0bfa996579dcc6d17a534741fb0a5da303a90579431259150de66b597251@54.147.31.250:30303","enode://f0f48a8781629f95ff02606081e6e43e4aebd503f3d07fc931fad7dd5ca1ba52bd849a6f6c3be0e375cf13c9ae04d859c4a9ae3546dc8ed4f10aa5dbb47d4998@34.226.134.117:30303"]
    ```

- To check if Heimdall is synced
    - On the remote machine/VM, run `curl localhost:26657/status`
    - In the output, `catching_up` value should be `false`

- Once Heimdall is synced, run
    - `sudo service bor start`

You have successfully set up a full node with Ansible.

:::note

If Bor presents an error of permission to data, run this command to make the Bor user the owner of the Bor files:

```bash
sudo chown bor /var/lib/bor
```

:::
## Logs

Logs can be managed by the `journalctl` linux tool. Here is a tutorial for advanced usage: [How To Use Journalctl to View and Manipulate Systemd Logs](https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs).

**Check Heimdall node logs**

```bash
journalctl -u heimdalld.service -f
```

**Check Bor Rest-server logs**

```bash
journalctl -u bor.service -f
```

## Ports and Firewall Setup

Open ports 22, 26656 and 30303 to world (0.0.0.0/0) on sentry node firewall.

You can use VPN to restrict access for port 22 as per your requirement and security guidelines.
