# Address

Validator Group Address

> 0xC05153EaAEAb67D5BAc1C25B1e5675Fb85B75a08

# How to Vote


## 1.) Download CLI

The CLI works best with Node 10:

```
npm install -g @celo/celocli
```

## 2.) Lock your cGLD from you address

Example Command:

```
celocli lockedgold:lock --from <YOUR-ADDRESS> --value <amount>
```
For more information on how to lock, refer to the [LockedGold Api](https://docs.celo.org/command-line-interface/lockedgold#lock) 


## 3.) Vote for G & S

Example Command:

```
celocli election:vote --for 0xC05153EaAEAb67D5BAc1C25B1e5675Fb85B75a08 --from <YOUR-ADDRESS> --value <AMOUNT>
```

For more information on how to vote, refer to the [Election Api](https://docs.celo.org/command-line-interface/election#vote)

