# Install Mdukey

This guide will explain how to install the `mdukeyd` and `mdukeycli` entrypoints
onto your system. With these installed on a server, you can participate in the
testnet as either a [Full Node](./join-testnet.md) or a
[Validator](./validator-setup.md).

## Install Go

Install `go` by following the [official docs](https://golang.org/doc/install).
Remember to set your `$GOPATH` and `$PATH` environment variables, for example:

```bash
mkdir -p $HOME/go/bin
echo "export GOPATH=$HOME/go" >> ~/.bash_profile
echo "export PATH=\$PATH:\$GOPATH/bin" >> ~/.bash_profile
source ~/.bash_profile
```

::: tip
**Go 1.13+** is required for the Cosmos SDK.
:::

## Install the binaries

Next, let's install the latest version of Mdukey. Make sure you `git checkout` the
correct [released version](https://github.com/mdukey/mdukey/releases).

```bash
git clone -b <latest-release-tag> https://github.com/mdukey/mdukey
cd mdukey && make install
```

If this command fails due to the following error message, you might have already set `LDFLAGS` prior to running this step.

```
# github.com/mdukey/mdukey/cmd/mdukeyd
flag provided but not defined: -L
usage: link [options] main.o
...
make: *** [install] Error 2
```

Unset this environment variable and try again.

```
LDFLAGS="" make install
```

> _NOTE_: If you still have issues at this step, please check that you have the latest stable version of GO installed.

That will install the `mdukeyd` and `mdukeycli` binaries. Verify that everything is OK:

```bash
$ mdukeyd version --long
$ mdukeycli version --long
```

`mdukeycli` for instance should output something similar to:

```shell
name: mdukey
server_name: mdukeyd
client_name: mdukeycli
version: 1.0.0
commit: 89e6316a27343304d332aadfe2869847bf52331c
build_tags: netgo,ledger
go: go version go1.12.5 darwin/amd64
```

### Build Tags

Build tags indicate special features that have been enabled in the binary.

| Build Tag | Description                                     |
| --------- | ----------------------------------------------- |
| netgo     | Name resolution will use pure Go code           |
| ledger    | Ledger devices are supported (hardware wallets) |

### Install binary distribution via snap (Linux only)

**Do not use snap at this time to install the binaries for production until we have a reproducible binary system.**

## Developer Workflow

To test any changes made in the SDK or Tendermint, a `replace` clause needs to be added to `go.mod` providing the correct import path.

- Make appropriate changes
- Add `replace github.com/mdukey/mdukey-sdk => /path/to/clone/mdukey-sdk` to `go.mod`
- Run `make clean install` or `make clean build`
- Test changes

## Next

Now you can join [the public testnet](./join-the-testnet.md) 
