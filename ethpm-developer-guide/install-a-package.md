# Getting Started with ethPM CLI

There are multiple options that allow you to start interacting with the ethPM ecosystem, but the easiest place to start is the [ethPM CLI](https://ethpm-cli.readthedocs.io/en/latest/). Full API documentation of these commands can be found [here](https://ethpm-cli.readthedocs.io/en/latest/commands.html).

## Setup

### Installing ethPM CLI

#### Homebrew \(recommended\)

* `brew update`
* `brew upgrade`
* `brew tap ethpm/ethpm-cli`
* `brew install ethpm-cli`

#### pypi

* Create your virtual environment
* Install the latest version from [pypi](https://pypi.org/project/ethpm-cli/) with `pip install ethpm-cli==x.x.x`

#### Docker

* `docker pull ethpm/ethpm:latest`
* `docker run ethpm/ethpm:latest`

### Setting up your environment vars

Before you can use ethPM CLI, you **must** provide an API key to interact with Infura. If you don't have an API key, you can sign up for one [here](https://infura.io). Then set your environment variable with 

```text
export WEB3_INFURA_PROJECT_ID="INSERT_KEY_HERE"
```

If you plan to [generate packages from Etherscan verified contracts](ethpm-and-etherscan-verified-contracts.md), you must also provide an API key for Etherscan.

```text
export ETHPM_CLI_ETHERSCAN_API_KEY="INSERT_KEY_HERE"
```

If you're using Docker to run ethPM CLI, you must pass Docker the environment variables and mount volumes, like so...

```text
docker run -i -e WEB3_INFURA_PROJECT_ID="INSERT_KEY_HERE" -v '/absolute/path/to/ethpm-cli/:/absolute/path/to/ethpm-cli/' -v '/$HOME/.local/share/ethpmcli/:/root/.local/share/ethpmcli/' ethpm/ethpm:latest list
```

### Setting up your private key

If you plan to use the CLI to send any transactions over an Ethereum network \(eg. deploying a new registry, releasing a package to a registry\), you must link a encrypted keyfile to sign these transactions. ethPM CLI uses [eth-keyfile](https://github.com/ethereum/eth-keyfile) to handle private keys. Follow the steps in the README to generate your encrypted keyfile. Make sure you don't lose the password, as you'll need to provide it for any tx-signing commands. Once you have your encrypted keyfile, you can link it to the ethPM CLI with the following command.

```text
ethpm auth --keyfile-path KEYFILE_PATH
```

## Commands

### Deploying a Registry

This will deploy a new instance of the [ERC1319 Solidity Registry](https://github.com/ethpm/solidity-registry) to the specified chain. The address signing the transaction will automatically be set as the `owner` address on the registry. The newly deployed registry will be set as the active registry. You can provide an alias, if you want to store a simple reference to the registry.

```text
ethpm registry deploy [-h] --chain-id CHAIN_ID 
                           --keyfile-password KEYFILE_PASSWORD 
                           [--alias ALIAS] 
```

### Generating an ethPM manifest

Thorough steps on how to generate a manifest for a local project can be found [here](https://ethpm-cli.readthedocs.io/en/latest/create.html).

To start a manifest wizard CLI prompt that will guide you through provided options allowed by the ethPM Specification.

```text
ethpm create manifest-wizard [-h] --project-dir PROJECT_DIR
```

To quickly generate a bare-bones manifest in one step, without any additional fields.

```text
ethpm create basic-manifest [-h] --package-name PACKAGE_NAME
                                 --package-version PACKAGE_VERSION
                                 --project-dir PROJECT_DIR
```

### Releasing a package

This will release a package version on the active registry.

```text
ethpm release [-h] --package-name PACKAGE_NAME
                   --package-version PACKAGE_VERSION
                   --manifest-uri MANIFEST_URI
                   --keyfile-password KEYFILE_PASSWORD
```

### Installing a package

This will install a package and its assets to a local `_ethpm_packages/` directory. You can provide a specific ethPM directory, otherwise the CLI will default to the ethPM directory under the current working directory. You can provide an alias for the package if you want a simple reference to the package.

```text
ethpm install [-h] manifest-uri [--alias ALIAS] [--ethpm-dir ETHPM_DIR]
```



