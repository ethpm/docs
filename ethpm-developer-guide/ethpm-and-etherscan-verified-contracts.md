---
description: Interact with the top 20 erc20 tokens right from your terminal.
---

# ethPM & Etherscan Verified Contracts

In this guide, we'll use [ethPM](https://web3py.readthedocs.io/en/stable/web3.pm.html) & [web3.py](https://github.com/ethereum/web3.py/) to instantly interact with any of the [top 20 erc20 ](https://etherscan.io/tokens?sortcmd=remove&sort=marketcap&order=desc)tokens with a couple lines of code from your terminal, **without** copy/pasting a single ABI!! In this example we'll be using the [DAI token](https://etherscan.io/token/0x89d24a6b4ccb1b6faa2625fe562bdd9a23260359), but all top 20 tokens are available as ethPM packages on the [erc20 package registry](../public-registry-directory.md). All you will need for this tutorial is an Infura API key, if you don't already have one, you can get one [here](https://infura.io).

### tl;dr

```python
> from web3.auto.infura import w3
> w3.pm.set_registry('erc20.snakecharmers.eth')
> dai_pkg = w3.pm.get_package("dai-dai")
> dai = dai_pkg.deployments.get_instance("DSToken")

# Now you can interact with dai like any erc20 tokens.
> dai.functions.totalSupply().call()
78512091351850936215202968
```

### Step by Step

#### Install Python

For this tutorial, you must have Python 3.6+ installed on your machine. Check out [this tutorial](https://realpython.com/installing-python/) for more info.

#### Create your directory & virtual environment

Virtual environments are best practice in Python world, so let's follow suit.

```bash
> mkdir ethpm-tutorial && cd ethpm-tutorial
> python -m venv venv
> export WEB3_INFURA_PROJECT_ID="YOUR_INFURA_API_KEY"
> source venv/bin/activate
```

#### Install web3.py & iPython

```bash
> pip install web3==5.0.1
> pip install ipython
> ipython
```

#### web3.py Setup

Some setup is required in `web3.py` before we can play with our Dai. The `enable_unstable_package_management_api` flag is a temporary security measure. While `web3.pm` is  in alpha, we want to be extra sure that users understand the API might change in the future. This precaution will be removed once the `web3.pm` API stabilizes in the next `web3.py` release.

```python
> from web3.auto.infura import w3
> w3.enable_unstable_package_management_api()
```

#### Set the active registry

We'll be using the erc20 registry located on the Ethereum mainnet at under the ENS name `erc20.snakecharmers.eth` which resolves to the address `0x16763EaE3709e47eE6140507Ff84A61c23B0098A` . You can explore the various erc20 tokens available on this registry on the [ethPM explorer](http://explorer.ethpm.com/browse/mainnet/erc20.snakecharmers.eth). Since our `w3` instance is already connected to mainnet, all we have to do is set the registry. 

```python
> w3.pm.set_registry('erc20.snakecharmers.eth')
```

#### Grab the Dai package

```python
> dai_package = w3.pm.get_package('dai-dai', '1.0.0')
```

#### Grab the Dai contract instance

Since the contract instance data is contained within the `dai-dai` package, it's quite simple to grab a [contract instance class](https://web3py.readthedocs.io/en/stable/contracts.html#) representing the Dai token contract. We will use the contract type `DSToken` to grab the Dai instance. 

```python
> dai = dai_package.deployments.get_instance("DSToken")
```

Sidenote: `dai_package` is an instance of `py-ethpm`'s [`Package` class](https://web3py.readthedocs.io/en/stable/ethpm.html#package). This class automatically filters available deployments that live on the connected `w3` instance. To access a package's deployments that live on a different blockchain, you can pass a new `w3` instance to `Package.update_w3()`.

#### Play with your Dai!

Now we have a contract instance representing the Dai contract. We can interact with any of its available functions. Of course, to send Dai, you must first own Dai! 

```python
> dai.functions.totalSupply().call()
78512091351850936215202968

> dai.functions.send("me.eth").transact()
```

You can follow these steps to interact with any of the erc20 token packages available in the connected registry.

## Generate your own Etherscan verified contract package

Each package in the above erc20 registry was created using the [`ethPM CLI`'s `create`](install-a-package.md#creating-a-package) command in combination with its corresponding [Etherscan URI](../uris.md#etherscan-uris). If you're the curious type, and want to re-generate the package locally, these steps can be used to package up any of Etherscan's verified contracts on any blockchain. 

To generate an Etherscan verified contract package, we'll use ethPM CLI. So you'll need to have that installed on your machine. Steps for how to get setup can be found [here](install-a-package.md).

Next, you'll need an Etherscan API key. If you don't already have an Etherscan API key, you can grab one [here](https://etherscan.io/register). Then set it as the following environment variable.

`export ETHPM_CLI_ETHERSCAN_API_KEY="YOUR_ETHERSCAN_KEY_HERE"`

ethPM CLI uses [Etherscan URIs](../uris.md#etherscan-uris) to identify the target verified contract. Once you have the address and chain ID of your verified contract, the URI simply follows this scheme.

`etherscan://[CONTRACT_ADDRESS]:[CHAIN_ID]`

i.e.

`etherscan://0x89d24A6b4CcB1B6fAA2625fE562bDD9a23260359:1`

Then, all you need to do is execute the following command, and the CLI will package up the verified contract, and write it to your local `_ethpm_packages/` directory. You can use any package name or package version that makes sense.

`ethpm install etherscan://0x89d24A6b4CcB1B6fAA2625fE562bDD9a23260359:1 --package-name my-package --package-version 1.0.0`

By default, this command will write the generated manifest to an `_ethpm_packages/` directory in the current working directory. If no such directory is found, it will automatically generate one. You can also pass the command an `--ethpm-dir` flag to target a specific directory. 

You can check out your newly created package by exploring the `_ethpm_packages/` directory, and you will also see it listed by calling `ethpm list`.

