# Sei-chain full node

* [Docs](https://docs.seinetwork.io/introduction/sei-ecosystem) - Sei Ecosystem

## Getting Started

Replace MONIKER and ACCOUNT_NAME with your variables

```
      MONIKER: "yournodenaame-seinode"
      ACCOUNT_NAME: "yourname-wallet"
```

Run docker-compose

```
docker-compose up -d
```

Get inside node

```
docker exec -ti sei ash
```

* [Continue from here](https://docs.seinetwork.io/nodes-and-validators/joining-testnets#3.-initialize-and-configure-moniker) - Initialize and configure moniker

```
seid init $MONIKER --chain-id sei-testnet-2 -o
```

Get genesis.json and addrbook.json

```
# Obtain the genesis file for sei-testnet-2:
curl https://raw.githubusercontent.com/sei-protocol/testnet/master/sei-testnet-2/genesis.json > ~/.sei/config/genesis.json
# Obtain the address book for sei-testnet-2
curl https://raw.githubusercontent.com/sei-protocol/testnet/master/sei-testnet-2/addrbook.json > ~/.sei/config/addrbook.json
```

Start seid and wait 5-10 minutes to begin syncronization

```
seid start
```

Create a new window with tmux or screen and get inside node again

```
docker exec -ti sei ash
```

Create a wallet 

```
seid keys add $ACCOUNT_NAME
```

Check full synchronization, the latest block should be the same as on the web page

* [Latest block](https://sei.explorers.guru) - Latest Block

```
seid status 2>&1 | jq .SyncInfo
```

After the full synchronization, ask coins in Discord faucet channel

* [Discord](https://discord.gg/YpYQ77Db) - Sei Discord

Check balance

```
WALLET_ADDRESS=$(seid keys show $ACCOUNT_NAME -a)
seid query bank balances $WALLET_ADDRESS
```

Run validator 

```
PUBKEY=$(seid tendermint show-validator)
WALLET_ADDRESS=$(seid keys show $ACCOUNT_NAME -a)
seid tx staking create-validator \
    --amount=1000000usei \
    --pubkey=$PUBKEY \
    --moniker=$MONIKER \
    --chain-id=$CHAIN_ID \
    --from=$ACCOUNT_NAME \
    --commission-rate="0.10" \
    --commission-max-rate="0.20" \
    --commission-max-change-rate="0.01" \
    --min-self-delegation="1" \
    --fees="2000usei"
```
