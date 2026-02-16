# 🚀 RepublicAI v0.3.0 Validator Upgrade Guide (Cosmovisor)

This guide provides step-by-step instructions to upgrade your **RepublicAI validator node** to **v0.3.0** on the **raitestnet_77701-1** network using **Cosmovisor**.

---

## 📌 Upgrade Specifications

- **Chain ID:** `raitestnet_77701-1`
- **Upgrade Height:** `326250`
- **Upgrade Name:** `v0.3.0`
- **Binary Target:** `republicd-linux-amd64`

---

# ✅ Step-by-Step Upgrade Instructions

---

## 1. Verify Upgrade Plan (Recommended)

Before upgrading, confirm the upgrade plan on-chain:

```
republicd query upgrade plan --node http://127.0.0.1:26657 -o json | jq
```
Expected:

name: v0.3.0

height: 326250

2. Create the Cosmovisor Upgrade Directory
Cosmovisor requires the binary to be placed inside the correct upgrade folder:

```
mkdir -p $HOME/.republicd/cosmovisor/upgrades/v0.3.0/bin
cd $HOME/.republicd/cosmovisor/upgrades/v0.3.0/bin
```
3. Download the v0.3.0 Binary
Download the official release binary:

```
wget -O republicd https://github.com/RepublicAI/networks/releases/download/v0.3.0/republicd-linux-amd64
```

4. Verify SHA256 Checksum
Ensure the binary is valid and not corrupted:

```
echo "bf0c88fda3ec40d8b991f87105c46ac6ddd7901d735213748de2c14e1b63a2a5  republicd" | sha256sum -c
```

Expected output:
```
makefile
republicd: OK
```

5. Make Binary Executable
```
chmod +x republicd
```
6. Confirm Binary Version
```
./republicd version
```
Expected:

```
0.3.0
```
⚡ Automatic Upgrade (Cosmovisor)
Once the binary is staged correctly, Cosmovisor will automatically switch to v0.3.0 at block:

```
326250
```
✅ No manual restart is required if Cosmovisor is running properly.

🔍 Post-Upgrade Verification
After the upgrade height has passed, verify your node is running v0.3.0:

```
$HOME/.republicd/cosmovisor/current/bin/republicd version
```
🛠 Troubleshooting
❗ Node Stuck at Upgrade Height
If your node stops at height 326250 with:

```
UPGRADE "v0.3.0" NEEDED
```
Follow these recovery steps.

1. Force Manual Cosmovisor Switch (Only If Needed)
Manually point Cosmovisor to the new upgrade folder:

```
ln -sfn $HOME/.republicd/cosmovisor/upgrades/v0.3.0 \
  $HOME/.republicd/cosmovisor/current
```
2. Refresh Persistent Peers
If your node is running but not syncing, update peers:
```
PEERS="4e14a1edc972ed3f4c03eae8434cb3997b342029@46.224.213.11:43656,\
c5f9653155d9095901c8044dc01fadf49212f350@45.143.198.6:26656,\
bb8dd41fc4731fd1b99bb054103c5c9526433bdc@149.5.246.217:43656,\
89f3b98f9428ce7c7bb6d48294dcceeb14446302@38.49.214.43:26656,\
87b1a77039b469eac7e3441ee14008cbed732ed9@38.49.214.87:26656,\
fb1f134e0dcd5c6c5719b318211598133fab46fb@154.12.118.199:26656"
```
```
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" \
  $HOME/.republicd/config/config.toml
```
3. Restart the Node Safely
Apply changes and check logs:

```
systemctl stop republicd
systemctl start republicd
```
```
journalctl -u republicd -f -o cat
```
⚠️ Important Notes
Non-Cosmovisor Users
If you are not using Cosmovisor, you must manually upgrade the binary after block 326250.

GPU Support
v0.3.0 includes optional GPU compute support.
Ensure NVIDIA drivers are installed if you plan to use GPU features:

```
nvidia-smi
```
✅ Upgrade Complete
Once your node is synced and running:

```
republicd version
```
Expected:

```
0.3.0
```
🎉 Your RepublicAI validator upgrade is successful!

