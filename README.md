# Persistence_mainnet
![ALT-logo](https://raw.githubusercontent.com/MaxFoton/Persistence_mainnet/main/Persistence.png)

`Github` https://github.com/persistenceOne

`Validator` maxfoton

`Delegate` https://ping.pub/persistence/staking/persistencevaloper1p509xkuc9a085rjetrp6gs2q6ca2246r59k9q3

`Valoper` persistencevaloper1p509xkuc9a085rjetrp6gs2q6ca2246r59k9q3

**RPC**
`RPC 95.165.89.222:26567`

**API**
`API 95.165.89.222:1326`

**gRPC** 
`gRPC 95.165.89.222:9392`

**Peer**
`Peer` 88497ab3bbbcc1e8545771f45020e738bcce590f@95.165.89.222:26565

***IBC Relayer GravityBridge***
`gravity13w9zq85yuk8jud83h2qncrzlsfq9s48k85t97c`
`persistence13w9zq85yuk8jud83h2qncrzlsfq9s48kdglw45`
`"channel-24", "channel-38"`

**Start whith state-sync**

```
sudo systemctl stop persistenceCore && persistenceCore tendermint unsafe-reset-all --home $HOME/.persistenceCore

curl https://raw.githubusercontent.com/MaxFoton/Persistence_mainnet/main/addrbook.json > ~/.persistenceCore/config/addrbook.json
```

```
SNAP_RPC=95.165.89.222:26567 && \
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) && \
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
```

```
sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.persistenceCore/config/config.toml
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.persistenceCore/config/config.toml
```

```
curl -o - -L https://raw.githubusercontent.com/MaxFoton/Persistence_mainnet/main/wasm.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.persistenceCore/
```

**Enable service and start**

```
sudo systemctl daemon-reload && sudo systemctl enable persistenceCore
sudo systemctl restart persistenceCore && sudo journalctl -fu persistenceCore -o cat
```
