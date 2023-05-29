---
id: supernets-how-to-stake
title: How to Stake on the Rootchain
sidebar_label: How to stake on the rootchain
description: "Learn how to stake on the rootchain."
keywords:
  - docs
  - polygon
  - supernets
  - stake
  - staking
  - MATIC
  - rootchain
---

In this section, we'll walkthrough how to stake test MATIC on the associated rootchain.

## i. Initial staking on the rootchain

Each validator needs to perform initial staking on the rootchain `StakeManager` contract. This is done using the `polygon-edge polybft stake` command. **Note that this command is for testing purposes only.**

<details>
<summary>Flags ↓</summary>

| Flag                          | Description                                        | Example                                  |
| -----------------------------| -------------------------------------------------- | ---------------------------------------- |
| `--amount `                     | The amount to stake                                | `--amount 5000000000000000000`           |
| `--chain-id`                    | The ID of the child chain                          | `--chain-id 100`                         |
| `--config `                     | The path to the SecretsManager config file         | `--config /path/to/config/file.yaml`     |
| `--data-dir`                    | The directory for the Polygon Edge data            | `--data-dir ./polygon-edge/data`         |
| `--jsonrpc`                     | The JSON-RPC interface                             | `--jsonrpc 0.0.0.0:8545`                |
| `--native-root-token `          | The address of the native root token               | `--native-root-token 0x<token_address>`  |
| `--stake-manager`               | The address of the stake manager contract          | `--stake-manager 0x<manager_address>`   |

</details>

In the following example command, we use the validator key and the rootchain `StakeManager` contract address that were generated earlier. We also set the staking amount to `1000000000000000000` which is equivalent to 1 token. The `--native-root-token` flag is used to specify the address of the native token of the root chain. In the case of Polygon, this is the MATIC token. You can obtain the address of the MATIC token by checking the network explorer or by querying the network using a tool like curl or web3.js.

<details>
<summary>Obtaining native root token address</summary>

For example, if you are using the Mumbai test network, you can obtain the address of the MATIC testnet token by sending a GET request to the Mumbai network's JSON-RPC endpoint:

```bash
curl <mumbai-rpc-endpoint> \
-X POST \
-H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"eth_contractAddress","params":["MaticToken"],"id":1}'
```

</details>

```bash
./polygon-edge polybft stake --data-dir ./test-chain-1 --chain-id 100 --amount 1000000000000000000 \
--stake-manager 0x6ceCFe1Db48Ab97fA89b06Df6Bd0bBdE6E64e6F7 --native-root-token 0x559Dd13d8A3CAb3Ff127c408f2A08A8a2AEfC56c
```

## ii. Finalize validator set on the rootchain

After all validators from the genesis block have performed initial staking on the rootchain, the final step required before starting the chain is to finalize the genesis validator set on the `SupernetManager` contract on the rootchain. This can be done using the `polygon-edge polybft supernet` command.

The deployer of the `SupernetManager` contract can specify their hex-encoded private key or use the `--data-dir` flag if they have initialized their secrets. If the `--enable-staking` flag is provided, validators will be able to continue staking on the rootchain. If not, genesis validators will not be able to update their stake or unstake, nor will newly registered validators after genesis be able to stake tokens on the rootchain. The enabling of staking can be done through this command or later after the Supernet starts.

In the following example command, we use a placeholder hex-encoded private key of the `SupernetManager` contract deployer. The addresses of the `SupernetManager` and `StakeManager` contracts are the addresses that were generated earlier. We also use the `--finalize-genesis` and `--enable-staking` flags to enable staking and finalize the genesis state.

```bash
   ./polygon-edge polybft supernet --private-key 0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef \
   --genesis /path/to/genesis/file \
   --supernet-manager 0x75aA024A2292A3FD3C17d67b54B3d00435437246 \
   --stake-manager 0x811068e4106f7A70D443684FF4927eC3940439Ec \
   --finalize-genesis --enable-staking
```
