Option 1: State Sync (Recommended)
State sync allows you to quickly sync from a recent snapshot instead of syncing from genesis.

# 1. Install republicd binary

```
VERSION="v0.1.0"

curl -L "https://media.githubusercontent.com/media/RepublicAI/networks/main/testnet/releases/${VERSION}/republicd-linux-amd64" -o /tmp/republicd

chmod +x /tmp/republicd

sudo mv /tmp/republicd /usr/local/bin/republicd
```


# 2. Initialize node

```
REPUBLIC_HOME="$HOME/.republicd"

republicd init <your-moniker> --chain-id raitestnet_77701-1 --home "$REPUBLIC_HOME"
```


# 3. Download genesis

```
curl -s https://raw.githubusercontent.com/RepublicAI/networks/main/testnet/genesis.json > "$REPUBLIC_HOME/config/genesis.json"
```


# 4. Configure state sync

```
SNAP_RPC="https://statesync.republicai.io"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height)

BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000))

TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)


sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \

s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \

s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \

s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" "$REPUBLIC_HOME/config/config.toml"
```


# 5. Configure persistent peers

```
PEERS="[email protected]:26656,[email protected]:26656"

sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" "$REPUBLIC_HOME/config/config.toml" 
```


# 6. Start node

```
republicd start --home "$REPUBLIC_HOME" --chain-id raitestnet_77701-1
```

Option 2: Full Sync from Genesis
# 1. Install republicd binary

```
VERSION="v0.1.0"

curl -L "https://media.githubusercontent.com/media/RepublicAI/networks/main/testnet/releases/${VERSION}/republicd-linux-amd64" -o /tmp/republicd

chmod +x /tmp/republicd

sudo mv /tmp/republicd /usr/local/bin/republicd
```


# 2. Initialize node

```
REPUBLIC_HOME="$HOME/.republicd"

republicd init <your-moniker> --chain-id raitestnet_77701-1 --home "$REPUBLIC_HOME"
```


# 3. Download genesis

```
curl -s https://raw.githubusercontent.com/RepublicAI/networks/main/testnet/genesis.json > "$REPUBLIC_HOME/config/genesis.json"
```


# 4. Configure persistent peers

```
PEERS="[email protected]:26656,[email protected]:26656"

sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" "$REPUBLIC_HOME/config/config.toml"
```


# 5. Start node

```
republicd start --home "$REPUBLIC_HOME" --chain-id raitestnet_77701-1
```

Option 3: Docker
Pull the published image:

```
docker pull ghcr.io/republicai/republicd:0.1.0
```


Initialize data and fetch genesis:

```
REPUBLIC_HOME="$HOME/.republicd"

mkdir -p "$REPUBLIC_HOME"
```

# Initialize (runs as root to create files)

```
docker run --rm \

  --user 0:0 \

  -v "$REPUBLIC_HOME:/home/republic/.republicd" \

  ghcr.io/republicai/republicd:0.1.0 \

  init my-node --chain-id raitestnet_77701-1 --home /home/republic/.republicd
```

# Download genesis

```
sudo curl -s https://raw.githubusercontent.com/RepublicAI/networks/main/testnet/genesis.json -o "$REPUBLIC_HOME/config/genesis.json"
```

Configure state sync (recommended for faster sync):

```
SNAP_RPC="https://statesync.republicai.io"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height)

BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000))

TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)


sudo sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \

s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \

s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \

s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" "$REPUBLIC_HOME/config/config.toml"


PEERS="[email protected]:26656,[email protected]:26656"

sudo sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" "$REPUBLIC_HOME/config/config.toml"
```

# Fix ownership to match container user (UID 1001)

```
sudo chown -R 1001:1001 "$REPUBLIC_HOME"
```

Run the node:

```
docker run -d --name republicd \

  --network host \

  -v "$REPUBLIC_HOME:/home/republic/.republicd" \

  ghcr.io/republicai/republicd:0.1.0 \

  start --home /home/republic/.republicd --chain-id raitestnet_77701-1
```

Note: --network host is required for proper P2P connectivity and state sync. Ports 26656, 26657, 1317, 9090, 8545, 8546 will be exposed on the host.

Check logs:

```
docker logs -f republicd
```

Becoming a Validator
Once your node is fully synced, you can create a validator:

# 1. Create or import a key

```
republicd keys add <key-name>
```

# OR import existing key

```
republicd keys add <key-name> --recover
```

# 2. Get testnet tokens from faucet (contact team)


# 3. Create validator

```
republicd tx staking create-validator \

  --amount=1000000000000000000000arai \

  --pubkey=$(republicd comet show-validator) \

  --moniker="<your-moniker>" \

  --chain-id=raitestnet_77701-1 \

  --commission-rate="0.10" \

  --commission-max-rate="0.20" \

  --commission-max-change-rate="0.01" \

  --min-self-delegation="1" \

  --gas=auto \

  --gas-adjustment=1.5 \

  --gas-prices="250000000arai" \

  --from=<key-name>
```

Note: Minimum self-delegation is 1000 RAI (1000000000000000000000 arai).

Useful Commands
```
Check Sync Status
republicd status | jq '.sync_info'
```

Check Validator Status
```
republicd query staking validator $(republicd keys show <key-name> --bech val -a)
```

Unjail Validator
```
republicd tx slashing unjail --from <key-name> --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5 --gas-prices 250000000arai
```

Delegate Tokens
```
republicd tx staking delegate <validator-address> <amount>arai --from <key-name> --chain-id raitestnet_77701-1 --gas auto --gas-adjustment 1.5 --gas-prices 250000000arai
```

Systemd Service
Create /etc/systemd/system/republicd.service:
```
[Unit]

Description=Republic Protocol Node

After=network-online.target



[Service]

User=ubuntu

ExecStart=/usr/local/bin/republicd start --home /home/ubuntu/.republicd --chain-id raitestnet_77701-1

Restart=always

RestartSec=3

LimitNOFILE=65535



[Install]

WantedBy=multi-user.target
```

Enable and start:
```
sudo systemctl daemon-reload

sudo systemctl enable republicd

sudo systemctl start republicd
```
