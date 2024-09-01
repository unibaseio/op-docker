# One-command containerized deployment for [opstack l2]("https://github.com/ethereum-optimism/optimism")

If you've deployed your own l2 following the [optimism tutorials](https://docs.optimism.io/builders/chain-operators/tutorials/create-l2-rollup), you'll notice there’s a lot of repetitive work involved. This project is perfect for you to quickly deploy an opstack l2 with a single command.

- [Prerequisites](#prerequisites)
- [Start deployment](#start-deployment)
- [Upgrade](#upgrade)
- [Reset deployment](#reset-deployment)

## Prerequisites

- Docker
- Docker compose plugin

> [!NOTE]
> Example: install on Amazon linux 2023:
>
> [Install and configure Docker on Amazon Linux 2023](https://gist.github.com/thimslugga/36019e15b2a47a48c495b661d18faa6d#install-and-configure-docker-on-amazon-linux-2023)
>
> [Install the Docker Compose v2 CLI Plugin](https://gist.github.com/thimslugga/36019e15b2a47a48c495b661d18faa6d#install-the-docker-compose-v2-cli-plugin)

## Start deployment

### Setup Environments

Rename `.env.example` to `.env`, then set the environment variables as needed.

```shell
# Repository configuration for optimism and op-geth
# If not set, default to the official optimism implementation

OPTIMISM_REPO_URL=https://github.com/ethereum-optimism/optimism.git
OPTIMISM_BRANCH_OR_COMMIT=v1.9.0        # optimism release version

OP_GETH_REPO_URL=https://github.com/ethereum-optimism/op-geth.git
OP_GETH_BRANCH_OR_COMMIT=v1.101315.3    # op-geth release version

# Admin account
ADMIN_PRIVATE_KEY=

# Batcher account
BATCHER_PRIVATE_KEY=

# Proposer account
PROPOSER_PRIVATE_KEY=

# Sequencer account
SEQUENCER_PRIVATE_KEY=

# Contract deployer account
DEPLOYER_PRIVATE_KEY=$ADMIN_PRIVATE_KEY

# The kind of RPC provider, used to inform optimal transactions receipts
# fetching. Valid options: alchemy, quicknode, infura, parity, nethermind,
# debug_geth, erigon, basic, any.
L1_RPC_KIND=alchemy

# RPC URL for the L1 network to interact with
L1_RPC_URL=xxxxxxx

L1_BLOCK_TIME=12

OP_NODE_L1_BEACON=http://testing.holesky.beacon-api.nimbus.team/

# The chain identifier for the L2 network
L2_CHAIN_ID=17000   # Sepolia: 11155111   Holesky: 17000

L2_BLOCK_TIME=2

```

### Run

```shell
./run
```

## Upgrade

You can find the latest optimism release here: [https://github.com/ethereum-optimism/optimism/releases](https://github.com/ethereum-optimism/optimism/releases), then update the branch or commit version.

```shell
OPTIMISM_REPO_URL=https://github.com/ethereum-optimism/optimism.git
OPTIMISM_BRANCH_OR_COMMIT=v1.9.0        # optimism release version

OP_GETH_REPO_URL=https://github.com/ethereum-optimism/op-geth.git
OP_GETH_BRANCH_OR_COMMIT=v1.101315.3    # op-geth release version
```

Finally, restart all opstack services

```shell
cd ~/op-docker
docker compose up -d
```

## Reset deployment

Stop all working opstack containers

```shell
docker stop $(docker ps -a -q -f "name=opstack-*")
```

Remove opstack images

```shell
docker rmi $(docker images -f "reference=opstack-*" -q)
```

Clean data

```shell
./clean
```

Redeploy

```shell
./run
```
