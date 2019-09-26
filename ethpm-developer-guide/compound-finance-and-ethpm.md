---
description: Earn interest on your DAI with ethPM and the Compound Protocol
---

# Compound Finance & ethPM

If you're like me, you have a hard time turning down free money. So why just let your crypto sit around and gather dust instead of putting it to work?

This guide will show you how to earn interest on your DAI with only a few lines of code in your terminal. You'll instantly start accumulating interest on your DAI by supplying it to the Compound protocol for others to borrow. We won't go deep into the Compound protocol, but you can learn more about it [here](https://compound.finance/developers#getting-started).

### Overview

* Use the DAI & Compound ethPM packages
* Approve the cDAI contract to transfer your DAI
* Exchange your DAI for cDAI \(aka supply your DAI to the Compound liquidity pool\)

### Prerequisites

* The private key for an account that owns DAI
* [`ethpm-cli` installed](install-a-package.md#installing-ethpm-cli) & [environment setup](install-a-package.md#setting-up-your-environment-vars)
* General understanding of [web3.py contract API](https://web3py.readthedocs.io/en/stable/contracts.html)

### Setup

First, let's get the two packages we'll need to supply our DAI to the Compound protocol.

1. [DAI](http://explorer.ethpm.com/manifest/QmTFxJbaJvpgASxxdqFPSvYr1XLWgXR9fv241jLXsELiXP)
   * package name: `dai-dai`
   * version: `1.0.0`
   * located on: [`erc20.snakecharmers.eth`](http://explorer.ethpm.com/browse/mainnet/erc20.snakecharmers.eth)\`\`
2. [Compound](http://explorer.ethpm.com/manifest/QmYvsyuxjj9mKmCvn3jrdfnaHYwFsyHXUu7kETrN4dBhE6) 
   * package name: `compound`
   * version: `1.0.0`
   * located on: [`defi.snakecharmers.eth`](http://explorer.ethpm.com/browse/mainnet/defi.snakecharmers.eth)\`\`

```python
from ethpm_cli.config import setup_w3

# `setup_w3(chain_id, private_key)` is a helper method from ethpm-cli
# that automatically connects to the designated chain via Infura and 
# authenticates your account to automatically sign all transactions
w3 = setup_w3(1, 'PRIVATE_KEY')

# Temporary security measure until PM api stabilizes
w3.enable_unstable_package_management_api()

# Grab the dai package from the snakecharmers erc20 registry
w3.pm.set_registry('erc20.snakecharmers.eth')
dai_pkg = w3.pm.get_package('dai-dai', '1.0.0')
dai = dai_pkg.deployments.get_instance('DSToken')

# Verify that everything's working
dai.functions.name().call()
> b'Dai Stablecoin v1.0\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'

# Grab the compound package from the snakecharmers defi registry
w3.pm.set_registry('defi.snakecharmers.eth')
compound_pkg = w3.pm.get_package('compound', '1.0.0')
cDai = compound_pkg.deployments.get_instance('cDAI')

# Verify that everything's working
cDai.functions.name().call()
> 'Compound Dai'
```

### Approve

Before we can supply our DAI to the Compound protocol, we have to approve the cDAI contract to transfer our DAI tokens and convert them into cDAI. This is a [standard procedure](https://medium.com/ethex-market/erc20-approve-allow-explained-88d6de921ce9) with many erc20-based systems. We'll use our `cDAI` contract instance for the address.

```python
# approve(spender: address, value: uint256)
dai.functions.approve(cDai.address, w3.toWei(100, 'ether').transact()
> 0x123...
```

### Supply

In the Compound v2 protocol, "minting" is analogous for supplying your DAI to the liquidity pool. Here we'll specify how many DAI we want to supply, and we start earning interest on the very next block after this tx is mined. Neato!

```python
# mint(value: uint256)
cDai.functions.mint(w3.toWei(1, 'ether')).transact()
> 0xabc...
```

### Check your stacks!

Give this a couple blocks before you accrue interest. But check daily to see the accumulated rewards of your DAI's hard work!

```python
cDai.functions.balanceOfUnderlying(w3.eth.defaultAccount).call()
> 1000000000000000001 # User's balance in DAI
```

Verify your supply & interest accrued with the [Compound dapp](https://app.compound.finance/).  You can also use these ethPM packages and your newly created contract instances to borrow DAI or any other supported feature of the [cToken API](https://compound.finance/developers/ctokens#ctokens). The `compound` package we use here also contains deployment information for all the other [mainnet cTokens](https://compound.finance/developers), so you can use the same package to lend/borrow REP, ETH, BAT, etc...

