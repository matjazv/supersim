<!-- omit in toc -->
# supersim fork (forked mode)

- [Overview](#overview)
- [Configuration](#configuration)
- [Notes](#notes)
  - [Fork height](#fork-height)
  - [Interoperability contracts](#interoperability-contracts)

## Overview

```sh
supersim fork
```

The supersim fork command simplifies the process of forking multiple chains in the Superchain ecosystem simultaneously. It determines the appropriate block heights for each chain and launches both the L1 and L2 chains based on these values.

If you're relying on contracts already deployed on testnet / mainnet chains, you can use fork mode to simulate and interact with the state of the chain without needing to re-deploy or modify the contracts.

Locally fork any of the available chains in a superchain network of the [superchain registry](https://github.com/ethereum-optimism/superchain-registry), default `mainnet` versions.

**Example startup logs**

```
Available Accounts
-----------------------
(0): 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
--- truncated for brevity ---

Private Keys
-----------------------
(0): 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
--- truncated for brevity ---

Orchestrator Config:
L1:
  Name: mainnet    Chain ID: 1    RPC: http://127.0.0.1:8545    LogPath: /var/folders/0w/ethers-phoenix/T/anvil-chain-1-1521250718
L2:
  Name: op    Chain ID: 10    RPC: http://127.0.0.1:9545    LogPath: /var/folders/0w/ethers-phoenix/T/anvil-chain-10
  Name: base    Chain ID: 8453    RPC: http://127.0.0.1:9546    LogPath: /var/folders/0w/ethers-phoenix/T/anvil-chain-8453
  Name: zora    Chain ID: 7777777    RPC: http://127.0.0.1:9547    LogPath: /var/folders/0w/ethers-phoenix/T/anvil-chain-7777777
```

## Configuration

```
NAME:
   supersim fork - Locally fork a network in the superchain registry

USAGE:
   supersim fork [command options]

OPTIONS:

          --l1.fork.height value              (default: 0)                       ($SUPERSIM_L1_FORK_HEIGHT)
                L1 height to fork the superchain (bounds L2 time). `0` for latest

          --chains value                                                         ($SUPERSIM_CHAINS)
                chains to fork in the superchain, mainnet options: [base, lyra, metal, mode, op,
                orderly, race, zora]. In order to replace the public rpc endpoint for a chain,
                specify the ($SUPERSIM_RPC_URL_<CHAIN>) env variable. i.e
                SUPERSIM_RPC_URL_OP=http://optimism-mainnet.infura.io/v3/<API-KEY>

          --network value                     (default: "mainnet")               ($SUPERSIM_NETWORK)
                superchain network. options: mainnet, sepolia, sepolia-dev-0. In order to
                replace the public rpc endpoint for the network, specify the
                ($SUPERSIM_RPC_URL_<NETWORK>) env variable. i.e
                SUPERSIM_RPC_URL_MAINNET=http://mainnet.infura.io/v3/<API-KEY>

          --interop.enabled                   (default: false)                   ($SUPERSIM_FORK_WITH_INTEROP)
                Enable interop in fork mode

          --l1.port value                     (default: 8545)                    ($SUPERSIM_L1_PORT)
                Listening port for the L1 instance. `0` binds to any available port

          --l2.starting.port value            (default: 9545)                    ($SUPERSIM_L2_STARTING_PORT)
                Starting port to increment from for L2 chains. `0` binds each chain to any
                available port

          --interop.autorelay                 (default: false)                   ($SUPERSIM_AUTORELAY)
                Automatically relay messages sent to the L2ToL2CrossDomainMessenger using
                account 0xa0Ee7A142d267C1f36714E4a8F75612F20a79720

          --aa.bundler                        (default: false)                   ($SUPERSIM_AA_BUNDLER)
                Run a ERC-4337 Bundler which sends transactions using account
                0x23618e81E3f5cdF7f54C3d65f7FBc0aBf5B21E8f

          --log.level value                   (default: INFO)                    ($SUPERSIM_LOG_LEVEL)
                The lowest log level that will be output

          --log.format value                  (default: text)                    ($SUPERSIM_LOG_FORMAT)
                Format the log output. Supported formats: 'text', 'terminal', 'logfmt', 'json',
                'json-pretty',

          --log.color                         (default: false)                   ($SUPERSIM_LOG_COLOR)
                Color the log output if in terminal mode

          --log.pid                           (default: false)                   ($SUPERSIM_LOG_PID)
                Show pid in the log

          --help, -h                          (default: false)
                show help
```

## Notes

### Fork height

The fork height is determined by L1 block height (default `latest`). This is then used to derive the corresponding L2 block to start from.

### Interoperability contracts

By default, interop contracts are not deployed on forked networks. To include them, run supersim with the `--interop.enabled` flag.

```sh
supersim fork --chains=op,base,zora --interop.enabled
```