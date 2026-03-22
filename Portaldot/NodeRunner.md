# 🟣 Portaldot Node Runner

<p align="center">
  <img src="https://img.shields.io/badge/build-passing-brightgreen" />
  <img src="https://img.shields.io/badge/version-v0.1-blue" />
  <img src="https://img.shields.io/github/contributors/portaldotVolunteer/Portaldot-node" />
  <img src="https://img.shields.io/github/license/portaldotVolunteer/Portaldot-node" />
</p>

<p align="center">
  Run a real Portaldot P2P network — from zero to two connected nodes
</p>

---

## 📸 Demo

<p align="center">
  <img src="https://raw.githubusercontent.com/johnwiard/Testnet-nodes/main/docs/demo.png" width="1000"/>
</p>

> 

---

## 🚀 Overview

**Portaldot Node Runner** lets you spin up a local P2P network in minutes.

* ⚡ Fast setup (no blockchain knowledge needed)
* 🌐 Real peer-to-peer networking
* 💰 Earn rewards (POT) by running nodes

---

## 📋 What You'll Do

1. Setup environment
2. Start **Alice (bootnode)**
3. Start **Bob (peer node)**
4. Validate connection (`1 peers`)

---

## 🖥️ Supported Environments

| Environment       | Best For     |
| ----------------- | ------------ |
| Windows (WSL2)    | Most users   |
| Ubuntu / Linux    | Native setup |
| GitHub Codespaces | No install   |
| Termux (Android)  | Mobile       |
| PowerShell        | Limited      |

---

# 🪟 Windows (WSL2)

## Install WSL

```bash
wsl --install -d Ubuntu
```

Restart if needed.

---

## Setup Node

```bash
wsl

cd ~
mkdir -p portaldot && cd portaldot

wget https://github.com/portaldotVolunteer/Portaldot-node/raw/main/portaldot-testnet-ubuntu.tar.gz

tar -xzvf portaldot-testnet-ubuntu.tar.gz
cd portaldot-testnet-ubuntu

chmod +x portaldot_dev
```

---

## ▶ Start Alice

```bash
./portaldot_dev --dev --alice --name YOUR_NAME --base-path /tmp/alice
```

Wait for:

```text
Local node identity is: 12D3KooW...
```

⚠️ Copy Peer ID
⚠️ Keep terminal open

---

## ▶ Start Bob

```bash
wsl
cd ~/portaldot/portaldot-testnet-ubuntu
```

```bash
./portaldot_dev --dev --bob --name YOUR_NAME_BOB \
  --base-path /tmp/bob \
  --port 30334 \
  --rpc-port 9945 \
  --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/PASTE_ALICE_PEER_ID
```

---

## ✅ Validate

```text
Idle (1 peers)
```

🎉 Your network is live!

---

# 🐧 Linux / Ubuntu

```bash
cd ~
mkdir -p portaldot && cd portaldot

wget https://github.com/portaldotVolunteer/Portaldot-node/raw/main/portaldot-testnet-ubuntu.tar.gz

tar -xzvf portaldot-testnet-ubuntu.tar.gz
cd portaldot-testnet-ubuntu

chmod +x portaldot_dev
```

Alice:

```bash
./portaldot_dev --dev --alice --name YOUR_NAME --base-path /tmp/alice
```

Bob:

```bash
cd ~/portaldot/portaldot-testnet-ubuntu

./portaldot_dev --dev --bob --name YOUR_NAME_BOB \
  --base-path /tmp/bob \
  --port 30334 \
  --rpc-port 9945 \
  --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/PASTE_ALICE_PEER_ID
```

---

# ☁️ GitHub Codespaces

```bash
cd ~

wget https://github.com/portaldotVolunteer/Portaldot-node/raw/main/portaldot-testnet-ubuntu.tar.gz

tar -xzvf portaldot-testnet-ubuntu.tar.gz
cd portaldot-testnet-ubuntu

chmod +x portaldot_dev
```

Alice:

```bash
./portaldot_dev --dev --alice --name YOUR_NAME --base-path /tmp/alice
```

Bob:

```bash
cd ~/portaldot-testnet-ubuntu

./portaldot_dev --dev --bob --name YOUR_NAME_BOB \
  --base-path /tmp/bob \
  --port 30334 \
  --rpc-port 9945 \
  --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/PASTE_ALICE_PEER_ID
```

---

# 📱 Termux (Android)

```bash
pkg update && pkg upgrade -y
pkg install wget tar -y
```

```bash
cd ~

wget https://github.com/portaldotVolunteer/Portaldot-node/raw/main/portaldot-testnet-ubuntu.tar.gz

tar -xzvf portaldot-testnet-ubuntu.tar.gz
cd portaldot-testnet-ubuntu

chmod +x portaldot_dev
```

---

# 🔧 Troubleshooting

### Lock file error

```bash
rm /tmp/alice/chains/dev/db/LOCK
rm /tmp/bob/chains/dev/db/LOCK
```

### Port already in use

```bash
pkill portaldot_dev
```

### Permission denied

```bash
chmod +x portaldot_dev
```

---

# 📸 Screenshot Requirement

```bash
./portaldot_dev --dev --alice --name your_username --base-path /tmp/alice
```

✔ Username must be visible
✔ Must show `1 peers`

---

# 🌐 UI Access

* https://polkadot.js.org/apps
* https://portaldot-playground.vercel.app

---

# 🏆 Contributing

We welcome contributions!

```bash
git fork
git clone
git checkout -b feature/your-feature
```

Submit a PR 🚀

---


# ⚡ Quick Reference

| Node  | Flag    | Port  | RPC  |
| ----- | ------- | ----- | ---- |
| Alice | --alice | 30333 | 9944 |
| Bob   | --bob   | 30334 | 9945 |

👉 **Rule: Start Alice first**

---

## ⭐ Support

If this helped you:

👉 Star the repo
👉 Share with others
👉 Run more nodes 🚀
