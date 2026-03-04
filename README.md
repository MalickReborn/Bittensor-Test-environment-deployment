# Bittensor-Test-environment-deployment
This repose showcase the setting up of a bittensor environment with Subtensor test node deployment  and a subnet and neurons


# 🔗 Bittensor Local Testnet Infrastructure  
### DevOps × Blockchain × Subnet Deployment

![Docker](https://img.shields.io/badge/Docker-Running-blue?logo=docker)
![Python](https://img.shields.io/badge/Python-3.10+-yellow?logo=python)
![Bittensor](https://img.shields.io/badge/Bittensor-Testnet-purple)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📖 Overview

This repository demonstrates how to:

- Run a **local Subtensor node** using Docker
- Configure a Python virtual environment
- Install and use **Bittensor CLI**
- Create and manage wallets
- Deploy a subnet on the Bittensor **test network**

This setup represents foundational infrastructure for building on Bittensor as a DevOps engineer.

---

## 🏗 Architecture Overview


+----------------------+
| Python venv |
| (bt_env) |
| bittensor-cli |
+----------+-----------+
|
v
+----------------------+
| Local Subtensor |
| Node (Docker) |
| testnet - lite |
+----------+-----------+
|
v
+----------------------+
| Wallet Creation |
| Subnet Deployment |
+----------------------+


---

## 📂 Project Structure


bittensorTraining/
│
├── bt_env/ # Python virtual environment
├── subtensor/ # Cloned subtensor repository
└── README.md # Documentation


---

## ⚙️ Prerequisites

- Docker
- Docker Compose
- Python 3.10+

### Install Docker (Ubuntu)

bash
```
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo systemctl start docker
```

Verify:

```
docker --version
docker compose version
```

### 🐍 Python Environment Setup

```
mkdir ~/bittensorTraining && cd ~/bittensorTraining
python3 -m venv bt_env
source bt_env/bin/activate
```

Install dependencies:

```
pip install bittensor
pip install bittensor-cli==9.1.0
```

### 🧠 Run Local Subtensor Node (Testnet)

 Clone repository:

```
git clone https://github.com/opentensor/subtensor.git
cd subtensor
```

### Clean Docker state (optional):

```
docker compose down --volumes
docker system prune -a --volumes -f
```

### Run lite node on testnet:

```
./scripts/run/subtensor.sh -e docker --network testnet --node-type lite
```
[![dockernod-tocreate.png](https://i.postimg.cc/0jw17wW3/dockernod-tocreate.png)](https://postimg.cc/sGsLrvR9)

Check if the container is correctly running

[![dockernoderunning.png](https://i.postimg.cc/bJFqx8zP/dockernoderunning.png)](https://postimg.cc/njB8xyjS)

Your local test node should now be running.

### 👛 Wallet Management

Create wallet:

```
btcli wallet create \
  --wallet.name Diego \
  --hotkey Diego_work \
  --network test
```
[![Screenshot-from-2026-03-03-14-21-29.png](https://i.postimg.cc/YqCscgfJ/Screenshot-from-2026-03-03-14-21-29.png)](https://postimg.cc/yW2PcDGP)

For this task i have funded my wallet using a [faucet for test tokens](https://app.minersunion.ai/testnet-faucet).

Check the wallet balance:

```
btcli wallet balance \
  --wallet.name Diego \
  --network test
```
[![check-balance.png](https://i.postimg.cc/Dwb4mjPS/check-balance.png)](https://postimg.cc/MnqpFY4w)

⚠️ Ensure your wallet has testnet TAO before creating a subnet.

### 🌐 Create a Subnet
```
btcli subnet create \
  --subnet-name Reddevil-Shelter \
  --wallet.name Diego \
  --wallet.hotkey Diego_work \
  --network test
```
[![subnet-creation.png](https://i.postimg.cc/7Zp7V8Z0/subnet-creation.png)](https://postimg.cc/QFJH8z5d)

[![Screenshot-from-2026-03-02-17-57-30.png](https://i.postimg.cc/V6XFMs9N/Screenshot-from-2026-03-02-17-57-30.png)](https://postimg.cc/9RFTHhn3)

List subnets:

```
btcli subnet list --network test
```

Start emissions on the subnet
To activate your subnet, beginning emissions and allowing staking, run:

```
btcli subnet start --netuid 282 --wallet.name Diego --network test
```
[![Screenshot-from-2026-03-03-17-22-38.png](https://i.postimg.cc/d0ZnYBTL/Screenshot-from-2026-03-03-17-22-38.png)](https://postimg.cc/jWKy6ycb)

You can check the emissions using once again:
```
btcli subnet list --network test
```
Now we have to register the neurons:

We' ll go first with the miner:
```
btcli subnet register --netuid 282 --wallet-name test-miner --hotkey test-miner_hotkey --network test

```
and we follow with the validator:
```
btcli subnet register --netuid 282 --wallet-name test-validator --hotkey test-validator_hotkey --network test
```

[![Screenshot-from-2026-03-03-17-33-57.png](https://i.postimg.cc/TPsSj1nW/Screenshot-from-2026-03-03-17-33-57.png)](https://postimg.cc/cKQX0s9s)

we then check everything is done correctly:
```
btcli subnet show --netuid 282 --network test
```

In order to submit weights, the validator has to aquire a work permit. We'll the request a permit by staking assets:

```
btcli stake add --netuid 2 --wallet-name test-validator --hotkey test-validator_hotkey --partial --network test
```

[![Screenshot-from-2026-03-03-17-37-10.png](https://i.postimg.cc/mD2xjJn2/Screenshot-from-2026-03-03-17-37-10.png)](https://postimg.cc/d7XStHvz)

and overview of the validator wallet status
```
btcli wallet overview --wallet.name test-validator --network test
```


### 🔄 Useful Commands

Deactivate virtual environment:

```
deactivate
```

Restart Docker node:

```
docker compose restart
```
### 🎯 What This Project Demonstrates

Infrastructure deployment using Docker

Blockchain node orchestration

Wallet lifecycle management

Subnet provisioning on Bittensor testnet

DevOps + Blockchain integration workflow

### 🚀 Next Steps

Register neurons


btcli wallet transfer --wallet.name Diego --destination test-miner --network test



Deploy miners / validators

Automate setup with CI/CD

Monitor node performance

Deploy on mainnet

