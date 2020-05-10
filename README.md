## Validator Group Address

> 0xC05153EaAEAb67D5BAc1C25B1e5675Fb85B75a08

## 1.) Setup a local accounts node

As a prerequisite, You will need Docker installed.

### 1a.) Environmental Variables

```
export CELO_IMAGE=us.gcr.io/celo-testnet/celo-node:rc1
export NETWORK_ID=42220
```

### 1b.) Pull the Docker Image

```
docker pull $CELO_IMAGE
```

### 1c.) Start your accounts-node

Fetch the genesis block and a list of other nodes in the network to connect to 

```
# On your local machine
docker run -v $PWD:/root/.celo --rm -it $CELO_IMAGE init /celo/genesis.json
export BOOTNODE_ENODES=`docker run --rm --entrypoint cat $CELO_IMAGE /celo/bootnodes`
```

Run the node

```
mkdir celo-accounts-node
cd celo-accounts-node
docker run --name celo-accounts -it --restart always -p 127.0.0.1:8545:8545 -v $PWD:/root/.celo $CELO_IMAGE --verbosity 3 --networkid $NETWORK_ID --syncmode full --rpc --rpcaddr 0.0.0.0 --rpcapi eth,net,web3,debug,admin,personal --bootnodes $BOOTNODE_ENODES
```

If you don't use a ledger, you can create a new account 

```
docker run -v $PWD:/root/.celo --rm -it $CELO_IMAGE account new
```

or move your existing key to the `keystore` folder.

For more information regarding starting your node, refer to the [docs](https://docs.celo.org/getting-started/rc1/running-a-validator-in-rc1#start-your-accounts-node).

## 2.) Download CLI

The CLI works best with Node 10:

```
npm install -g @celo/celocli
```

## 3.) Lock your cGLD

Example Command:

```
celocli lockedgold:lock --from <YOUR-ADDRESS> --value <AMOUNT>
```
For more information on how to lock, refer to the [LockedGold Api](https://docs.celo.org/command-line-interface/lockedgold#lock).


## 4.) Vote for Goods & Services

Example Command:

```
celocli election:vote --for 0xC05153EaAEAb67D5BAc1C25B1e5675Fb85B75a08 --from <YOUR-ADDRESS> --value <AMOUNT>
```

For more information on how to vote, refer to the [Election Api](https://docs.celo.org/command-line-interface/election#vote).

## 5.) Activate your Votes

Exmaple Command

```
# Note that this command will wait for the next epoch transition, which may be up to 24 hours in the future. Consider running the command in a Screen session or Tmux window.
celocli election:activate --from <YOUR-ADDRESS> --wait
```

For more information on how to activate, refer to the [Election Api](https://docs.celo.org/command-line-interface/election#activate).


