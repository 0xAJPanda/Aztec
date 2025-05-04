![Aztec Banner](https://raw.githubusercontent.com/0xAJPanda/Aztec/main/bannerAztec.png)

# 🚀 Aztec Sequencer Node – Quick Start
...


> Earn the `Apprentice` role by running a Sequencer Node on Aztec Testnet.

---

## ✅ Requirements

- **CPU:** 8 cores  
- **RAM:** 16 GB  
- **Disk:** 100 GB SSD  
- **OS:** Ubuntu (or WSL on Windows)  
- **Ethereum Wallet:** With private key + Sepolia ETH  

---

## 🔧 1. Install Tools & Docker

```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```

```bash
sudo apt update && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker
sudo docker run hello-world

sudo systemctl enable docker
sudo systemctl restart docker
```

---

## 🧱 2. Install Aztec CLI

```bash
bash -i <(curl -s https://install.aztec.network)
```

Then:
```bash
aztec
aztec-up alpha-testnet
```

---

## 🌐 3. Get Sepolia URLs

- **RPC URL (Free):** [Alchemy](https://dashboard.alchemy.com/)  
- **Beacon URL (Free):** `https://rpc.drpc.org/eth/sepolia/beacon`


- ** ANKR Paid option : [ANKR](https://www.ankr.com/rpc/?utm_referral=q47m74p398)

---

## 🏃 4. Run the Node

```bash
screen -S aztec

aztec start --node --archiver --sequencer \
--network alpha-testnet \
--l1-rpc-urls RPC_URL \
--l1-consensus-host-urls BEACON_URL \
--sequencer.validatorPrivateKey 0xPRIVATE_KEY \
--sequencer.coinbase 0xYOUR_ADDRESS \
--p2p.p2pIp YOUR_SERVER_IP
--p2p.maxTxPoolSize 1000000000 \
```

🔁 Replace variables with your actual info.

---

## 🏅 5. Claim `Apprentice` Role

### Step 1 – Get block number
```bash
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' \
http://localhost:8080 | jq -r ".result.proven.number"
```

### Step 2 – Get proof
```bash
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["BLOCK","BLOCK"],"id":67}' \
http://localhost:8080 | jq -r ".result"
```

### Step 3 – Go to Discord  
Type:
```
/operator start
```
> Submit your address, block number, and proof

---

## 🗳️ 6. Register as Validator

```bash
aztec add-l1-validator \
--l1-rpc-urls RPC_URL \
--private-key 0xPRIVATE_KEY \
--attester 0xYOUR_ADDRESS \
--proposer-eoa 0xYOUR_ADDRESS \
--staking-asset-handler 0xF739D03e98e23A7B65940848aBA8921fF3bAc4b2 \
--l1-chain-id 11155111
```

---

## 🧹 Optional: Fix Sync Error

```bash
rm -r ~/.aztec/alpha-testnet
aztec start ...
```

---

✅ Done! You’re now part of the Aztec Network.
