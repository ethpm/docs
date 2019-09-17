---
description: How to get the most out of ethPM's plugin for Remix.
---

# ethPM & Remix IDE

## What can I do with the ethPM Remix plugin?

* Generate an ethPM package from smart contracts in your Remix editor
* Publish generated ethPM packages to IPFS or download them locally
* Import smart contract source files from ethPM packages directly into the Remix editor
* Import deployed contract instances from ethPM packages, and interact with them directly in Remix

## How to create a package?

1. Compile the contract you want to package up with the `Solidity Compiler` plugin.
   *  Currently, the Vyper compiler plugin is **not** supported.
2. Select the contract types you would like to include in the package.
3. Enter your package name and version \(both are required to generate a valid ethPM package\).
4. Enter any metadata you would like to include in your package.
5. Click `Generate Manifest`.
6. Wait a bit while the plugin generates your manifests, and publishes it to IPFS \(via the Infura gateway\).
7. Click `Manifest Preview` to view the manifest in the ethPM explorer 
   * Note. this does not mean that your manifest has been published to a registry - it's simply a pretty view of your beautiful manifest json.
8. Click `Download Raw Manifest` to save a local copy of the generated manifest json.
   * Note - when downloaded, the json file has escaped characters which means it's an invalid ethPM manifest. To make this a valid ethPM manifest, you must remove the escape characters. Sorry for the inconvenience, we're working to eliminate this hurdle asap.

#### Next steps?

* Use the ethPM CLI to [deploy your own ethPM  registry](install-a-package.md#deploying-a-registry).
* Use the ethPM CLI to [release your newly created package](install-a-package.md#releasing-a-package), using the generated IPFS url.

## How to import a manifest?

* Click on `Import a Package`
* Enter the IPFS url associated with the desired package.
* Wait a second while the plugin fetches the manifest from IPFS.
* Click on `Import` to import any source file found in the manifest to Remix's in-browser filesystem.
* Click on `Import ABI` to import the ABI for any contract deployment found in the manifest to Remix's in-browser filesystem.

## How to interact a deployment?

* Follow the steps in `How to import a manifest?` \(above\) 
* Click on `Import ABI` for whatever contract deployment you want to interact with. 
* Activate the `Deploy and Run Transactions` plugin in Remix.
* Select `Injected Web3` in the `Environment` dropdown. 
* Make sure Metamask is connected to whatever chain your target deployment is located on.
* Copy and paste the address of the target deployment into `At Address`.
* Voila, now you can interact with the target deployment right in the Remix editor.

