## Validator Group Address

> 0xC05153EaAEAb67D5BAc1C25B1e5675Fb85B75a08

# Guide to Voting for G&S

## Setup a local accounts node

As a prerequisite, you will need Docker installed.

### Environmental Variables

```
export CELO_IMAGE=us.gcr.io/celo-testnet/celo-node:rc1
export NETWORK_ID=42220
```

### Pull the Docker image

```
docker pull $CELO_IMAGE
```

### Start your `accounts` node

Fetch the genesis block and a list of other nodes in the network to connect with:

```
mkdir celo-accounts-node
cd celo-accounts-node
# On your local machine
docker run -v $PWD:/root/.celo --rm -it $CELO_IMAGE init /celo/genesis.json
export BOOTNODE_ENODES=`docker run --rm --entrypoint cat $CELO_IMAGE /celo/bootnodes`
```

Run the node:

```
docker run --name celo-accounts -it --restart always -p 127.0.0.1:8545:8545 -v $PWD:/root/.celo $CELO_IMAGE --verbosity 3 --networkid $NETWORK_ID --syncmode full --rpc --rpcaddr 0.0.0.0 --rpcapi eth,net,web3,debug,admin,personal --bootnodes $BOOTNODE_ENODES
```

If you don't use a Ledger hardware wallet, you can create a new account:

```
docker run -v $PWD:/root/.celo --rm -it $CELO_IMAGE account new
```

...or move your existing key to the `keystore` folder.

For more information regarding starting your node, refer to the [docs](https://docs.celo.org/getting-started/rc1/running-a-validator-in-rc1#start-your-accounts-node).

## Download CLI

The CLI works best with Node 10:

```
npm install -g @celo/celocli
```

## Lock your CELO

Example address command:

```
celocli lockedgold:lock --from <YOUR-ADDRESS> --value <AMOUNT>
```

Example `releasegold` command:

```
celocli releasegold:locked-gold --contract <YOUR-RG-ADDRESS> --action lock --value <AMOUNT>
```

*For more information on how to lock, refer to the [LockedGold API](https://docs.celo.org/command-line-interface/lockedgold#lock).*

## Create and authorize a vote signer key [optional step for `ReleaseGold` contracts]

Generate your vote signer key:

```
docker run -v $PWD:/root/.celo --rm -it $CELO_IMAGE account new
```

Generate your vote signer signature:

```
docker run -v $PWD:/root/.celo --rm -it $CELO_IMAGE --nousb account proof-of-possession <YOUR-VOTE-SIGNER-ADDRESS> <YOUR-RG-ADDRESS>
```

Authorize your new address as a vote signer:

```
celocli releasegold:authorize --contract <YOUR-RG-ADDRESS> --role vote --signature <VOTE-SIGNER-SIGNATURE> --signer <VOTE-SIGNER-ADDRESS>
```

*For more information on how to authorize, refer to the [ReleaseGold API](https://docs.celo.org/command-line-interface/releasegold#release-gold).*

## Vote for Goods & Services

Example command:

```
celocli election:vote --for 0xC05153EaAEAb67D5BAc1C25B1e5675Fb85B75a08 --from <YOUR-ADDRESS-OR-RG-SIGNER> --value <AMOUNT>
```

*For more information on how to vote, refer to the [Election API](https://docs.celo.org/command-line-interface/election#vote).*

## Activate your votes

Example command:

```
# Note that this command will wait for the next epoch transition, which may be up to 24 hours in the future. Consider running the command in a Screen session or Tmux window.
celocli election:activate --from <YOUR-ADDRESS-OR-RG-SIGNER> --wait
```

*For more information on how to activate, refer to the [Election API](https://docs.celo.org/command-line-interface/election#activate).*

