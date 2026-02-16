RepublicAI v0.3.0 Validator Upgrade Guide (Cosmovisor) This guide provides step-by-step instructions to upgrade your RepublicAI validator node to v0.3.0 on the raitestnet_77701-1 network using Cosmovisor.

Upgrade Specifications Upgrade Height: 326250

Upgrade Name: v0.3.0

Binary Target: republicd-linux-amd64

Step-by-Step Installation

Prepare the Upgrade Directory Create the necessary folder structure for Cosmovisor to recognize the new version.
```
mkdir -p $HOME/.republicd/cosmovisor/upgrades/v0.3.0/bin
```
Navigate to the Target Folder Move into the newly created bin directory.
```
cd $HOME/.republicd/cosmovisor/upgrades/v0.3.0/bin
```
Download the New Binary Fetch the official v0.3.0 release from the RepublicAI GitHub repository.
```
wget -O republicd https://github.com/RepublicAI/networks/releases/download/v0.3.0/republicd-linux-amd64
```
Verify Integrity (SHA256 Checksum) Ensure the downloaded file is not corrupted or tampered with.
```
echo "bf0c88fda3ec40d8b991f87105c46ac6ddd7901d735213748de2c14e1b63a2a5  republicd" | sha256sum -c
```
Expected Output: republicd: OK

Set Execution Permissions Grant the binary the necessary permissions to run.
```
chmod +x republicd
```
Verify Version Confirm that the binary is indeed the correct version.
```
./republicd version
```
Post-Upgrade Protocol Once these steps are completed, Cosmovisor will automatically switch to the v0.3.0 binary exactly at block 326250.No manual restart will be required if Cosmovisor is running as a background service.

Post-Upgrade Verification After the network reaches the upgrade height, you can verify your active version with:

```
root/.republicd/cosmovisor/current/bin/republicd version`
``
🛠️ Troubleshooting: If your node gets stuck at block 326250 Sometimes Cosmovisor might fail to switch binaries automatically. If you see your block height stuck at 326250 and get UPGRADE NEEDED or Connection Refused errors, apply these steps:

Force Upgrade (Manual Link) Manually point Cosmovisor to the new v0.3.0 binary:
```
ln -sfn $HOME/.republicd/cosmovisor/upgrades/v0.3.0 $HOME/.republicd/cosmovisor/current
```
Refresh Peer Connections If your node is running but not syncing new blocks, update your peer list to connect with other v0.3.0 nodes:
```
PEERS="4e14a1edc972ed3f4c03eae8434cb3997b342029@46.224.213.11:43656,c5f9653155d9095901c8044dc01fadf49212f350@45.143.198.6:26656,bb8dd41fc4731fd1b99bb054103c5c9526433bdc@149.5.246.217:43656,89f3b98f9428ce7c7bb6d48294dcceeb14446302@38.49.214.43:26656,87b1a77039b469eac7e3441ee14008cbed732ed9@38.49.214.87:26656,fb1f134e0dcd5c6c5719b318211598133fab46fb@154.12.118.199:26656"
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.republicd/config/config.toml
```
Restart the Service Apply changes and check the logs:
```
systemctl restart republicd
journalctl -u republicd -f -o cat`
``
⚠️ Important Notes Non-Cosmovisor Users: If you are not using Cosmovisor, you must perform these steps manually ONLY AFTER block 326250 is reached.

GPU Support: This version enables GPU computing. Ensure your NVIDIA drivers are correctly installed (nvidia-smi) to benefit from aggressive rewards.
