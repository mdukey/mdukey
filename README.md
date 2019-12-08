# Mdukey

This repository hosts `Mdukey`, the first implementation of the Mdukey.

**Note**: Requires [Go 1.13+](https://golang.org/dl/)


## Quick Start

```
make install
```

## Local Testnet

```
mdukeyd  init --chain-id=testing testing

mdukeycli  keys add validator
  
mdukeyd  add-genesis-account $(mdukeycli keys show validator -a) 190000000000000umdu,1000000000validatortoken
  
mdukeyd gentx --name validator

mdukeyd collect-gentxs

mdukeyd start

// start lcd server
mdukeycli rest-server --chain-id=testing
```