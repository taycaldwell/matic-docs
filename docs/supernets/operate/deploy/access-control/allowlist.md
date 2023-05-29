---
id: supernets-how-to-allowlist
title: How to Allowlist Validators
sidebar_label: How to allowlist validators
description: "Learn how to allowlist addresses on a Supernet."
keywords:
  - docs
  - polygon
  - allowlist
  - allow
  - access
  - access control
  - block
  - blocklist
---

In this section, we'll look at how to allowlist validators on the rootchain.

## i. Allowlist validators on the rootchain

The `CustomSupernetManager` contract on the rootchain is responsible for managing the PolyBFT network.

Before validators can be registered on the `CustomSupernetManager` contract on the rootchain, they need to be allowlisted by the deployer of the `CustomSupernetManager` contract. Validators can register themselves, or a registrator can register them on their behalf. Once registered, validators can stake and start validating transactions.

This can be done using the `polygon-edge polybft whitelist-validators` command. The deployer can specify the hex-encoded private key of the `CustomSupernetManager` contract deployer or use the `--data-dir` flag if they have initialized their secrets.

<details>
<summary>Flags ↓</summary>

| Flag              | Description                                                                                      | Example                                     |
| -----------------| ------------------------------------------------------------------------------------------------| ------------------------------------------- |
| `--private-key`     | Hex-encoded private key of the account that deploys the SupernetManager contract                | `--private-key <hex_encoded_rootchain_account_private_key_of_CustomSupernetManager_deployer>`             |
| `--addresses`       | Comma-separated list of hex-encoded addresses of validators to be whitelisted                   | `--addresses 0x8a98f47a9820e3f3a6C16f44194F1d7eCCe3A110,0x8a98f47a9820e3f3a6C16f44194F1d7eCCe3A110` |
| --supernet-manager| Address of the SupernetManager contract on the rootchain                                        | `--supernet-manager 0x3c6f8c6Fd90b2Bee1E78E2B2D1e7aB6cFf9Dc113` |
| `--data-dir`        | Directory for the Polygon Edge data if the local FS is used                                     | `--data-dir ./polygon-edge/data`             |
| `--jsonrpc`         | JSON-RPC interface                                                                              | `--jsonrpc 0.0.0.0:8545`                    |
| `--config`          | Path to the SecretsManager config file. If omitted, the local FS secrets manager is used        | `--config /path/to/config/file.yaml`        |

</details>

In the following example command, we are using a placeholder private key for the `CustomSupernetManager` contract deployer. 

> If running the demo geth instance, the test account private key has been hardcoded: `aa75e9a7d427efc732f8e4f1a5b7646adcc61fd5bae40f80d13c8419c9f43d6d`.

The `--addresses` flag is a comma-separated list of the first two validators generated earlier. The `--supernet-manager` flag specifies the actual `CustomSupernetManager` contract address that was deployed.

```bash
./polygon-edge polybft whitelist-validators \
  --private-key 0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef \
  --addresses 0x61324166B0202DB1E7502924326262274Fa4358F,0xFE5E166BA5EA50c04fCa00b07b59966E6C2E9570 \
  --supernet-manager 0x75aA024A2292A3FD3C17d67b54B3d00435437246
```

## ii. Register validators on the rootchain

Each validator needs to register themselves on the `CustomSupernetManager` contract. This is done using the `polygon-edge polybft register-validator` command. **Note that this command is for testing purposes only.**

<details>
<summary>Flags ↓</summary>

| Flag                          | Description                                                                                                       | Example                                                |
| -----------------------------| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| `--config`                      | Path to the SecretsManager config file. If omitted, the local FS secrets manager is used.                          | `--config /path/to/config/file.yaml`                   |
| `--data-dir`                    | The directory path where the new validator key is stored.                                                         | `--data-dir /path/to/validator1`                       |                                                      |
| `--jsonrpc`                     | The JSON-RPC interface. Default is `0.0.0.0:8545`.                                                                 | `--jsonrpc 0.0.0.0:8545`                              |
| `--supernet-manager`            | Address of the SupernetManager contract on the rootchain.                                                          | `--supernet-manager 0x75aA024A2292A3FD3C17d67b54B3d00435437246`      |

</details>

```bash
./polygon-edge polybft register-validator --data-dir ./test-chain-1 \
--supernet-manager --supernet-manager 0x75aA024A2292A3FD3C17d67b54B3d00435437246
```
