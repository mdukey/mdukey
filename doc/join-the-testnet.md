# Join the testnet

::: warning
**You need to [install mdukey](./installation.md) before you go further**
:::

## Setting Up a New Node

These instructions are for setting up a brand new full node from scratch.

First, initialize the node and create the necessary config files:

```bash
mdukeyd init <your_custom_moniker>
```

::: warning Note
Monikers can contain only ASCII characters. Using Unicode characters will render your node unreachable.
:::

You can edit this `moniker` later, in the `~/.mdukeyd/config/config.toml` file:

```toml
# A custom human readable name for this node
moniker = "<your_custom_moniker>"
```

You can edit the `~/.mdukeyd/config/mdukeyd.toml` file in order to enable the anti spam mechanism and reject incoming transactions with less than the minimum gas prices:

```
# This is a TOML config file.
# For more information, see https://github.com/toml-lang/toml

##### main base config options #####

# The minimum gas prices a validator is willing to accept for processing a
# transaction. A transaction's fees must meet the minimum of any denomination
# specified in this config (e.g. 10umdu).

minimum-gas-prices = ""
```

Your full node has been initialized! 

## Genesis & Seeds

### Copy the Genesis File

Fetch the testnet's `genesis.json` file into `mdukeyd`'s config directory.

```bash
mkdir -p $HOME/.mdukeyd/config
curl https://raw.githubusercontent.com/mdukey/testnets/master/lastest/genesis.json > $HOME/.mdukeyd/config/genesis.json
```

Note we use the `latest` directory in the [testnets repo](https://github.com/mdukey/testnets) which contains details for the testnet like the latest version and the genesis file. 

To verify the correctness of the configuration run:

```bash
mdukeyd start
```

### Add Seed Nodes

Your node needs to know how to find peers. You'll need to add healthy seed nodes to `$HOME/.mdukeyd/config/config.toml`. The [`testnets repo`](https://github.com/mdukey/testnets) repo contains links to some seed nodes.

If those seeds aren't working, you can find more seeds and persistent peers on a Mudkey explorer (a list can be found on the [the-page-coming-soon](https://mdukey.network)). 

## A Note on Gas and Fees

::: warning
On Mudkey Hub testnet, the accepted denom is `umdu`, where `1mdu = 1.000.000umdu`
:::

Transactions on the Mudkey Hub network need to include a transaction fee in order to be processed. This fee pays for the gas required to run the transaction. The formula is the following:

```
fees = ceil(gas * gasPrices)
```

The `gas` is dependent on the transaction. Different transaction require different amount of `gas`. The `gas` amount for a transaction is calculated as it is being processed, but there is a way to estimate it beforehand by using the `auto` value for the `gas` flag. Of course, this only gives an estimate. You can adjust this estimate with the flag `--gas-adjustment` (default `1.0`) if you want to be sure you provide enough `gas` for the transaction. 

The `gasPrice` is the price of each unit of `gas`. Each validator sets a `min-gas-price` value, and will only include transactions that have a `gasPrice` greater than their `min-gas-price`. 

The transaction `fees` are the product of `gas` and `gasPrice`. As a user, you have to input 2 out of 3. The higher the `gasPrice`/`fees`, the higher the chance that your transaction will get included in a block. 

::: tip
For testnet, the recommended `gas-prices` is `0.025umdu. 
::: 

## Set `minimum-gas-prices`

Your full-node keeps unconfirmed transactions in its mempool. In order to protect it from spam, it is better to set a `minimum-gas-prices` that the transaction must meet in order to be accepted in your node's mempool. This parameter can be set in the following file `~/.mdukeyd/config/mdukeyd.toml`.

The initial recommended `min-gas-prices` is `0.025umdu but you might want to change it later. 

## Run a Full Node

Start the full node with this command:

```bash
mdukeyd start
```

Check that everything is running smoothly:

```bash
mdukeycli status
```

View the status of the network with the [Mdukey Explorer](https://scan.mdukey.org). 

## Export State

Mdukey can dump the entire application state to a JSON file, which could be useful for manual analysis and can also be used as the genesis file of a new network.

Export state with:

```bash
mdukeyd export > [filename].json
```

You can also export state from a particular height (at the end of processing the block of that height):

```bash
mdukeyd export --height [height] > [filename].json
```

If you plan to start a new network from the exported state, export with the `--for-zero-height` flag:

```bash
mdukeyd export --height [height] --for-zero-height > [filename].json
```

## Upgrade to Validator Node

You now have an active full node. What's the next step? You can upgrade your full node to become a Mudkey Validator. The top 100 validators have the ability to propose new blocks to the Mudkey Zone. Continue onto [the Validator Setup](./validator-setup.md).
