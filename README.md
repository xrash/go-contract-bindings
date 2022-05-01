# geth-contract-bindings

This repo is a boilerplate for generating Go source able to call methods on smart contracts from the Uniswap v2 project, through a program called `abigen`, which is part of the `go-ethereum` project.

## Step by step

1. First of all, clone this repo on your machine.

2. Now we need to download the source code of all the contracts from Uniswap V2. Inside this directory, run:

```bash
$ git clone git@github.com:Uniswap/v2-core.git
```

Note that the `.gitignore` from this directory is prepared to ignore `v2-core`.

3. Make sure you got Docker installed and `dockerd` running.

4. Now we need to compile the Solidity code into its ABI. Since we care about the version of the compiler (because the contracts from Uniswap V2 specify the version we need), we're going to use a dockerized version of `solc` at version 0.5.16. Still in this directory, run:

```bash
$ docker run --rm -v $(pwd):/root ethereum/solc:0.5.16 --abi /root/v2-core/contracts/UniswapV2Pair.sol -o /root/build
```

It should create a directory called `build` containing our ABI files.


5. Now we need to generate Go code from the ABI. Make sure you have `abigen` installed and then, still in this directory, run:

```bash
$ abigen --pkg uniswap_v2 --abi build/UniswapV2Pair.abi > uniswap_v2_pair.go
```

Finally, `uniswap_v2_pair.go` is the Go source that you can import in your Go code and call methods from the Pair smart contract from the Uniswap V2 project.
