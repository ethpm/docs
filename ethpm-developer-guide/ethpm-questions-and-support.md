# FAQs

#### Why should I care about ethPM?

Nick Gnidan from Truffle wrote a beautiful [blog](https://medium.com/coinmonks/ethpm-smart-contract-packages-for-developers-81c77481c491) which answers this. 

#### Why isn't there one central registry?

While having a single, centralized registry can be convenient for discovering new packages, there are some downsides, such as a limited feature set and maintenance overhead. ethPM v2 uses a federated registry model to overcome these limitations. In a world where code controls large amounts of money, malicious packages pose a serious threat. Forcing package authors to deploy and maintain their own authorized registry minimizes the chance that a malicious release finds its way into a popular package. Furthermore, the overhead of maintaining a central registry is significant, and more sustainable if distributed amongst package authors. Finally, defining a standard which can be extended allows devs to write custom logic into their package registry to achieve whatever goal they have in mind allows for new ideas to be experimented with and bloom.

#### What about ethPM v1 packages?

ethPM v2 made breaking changes from ethPM v1, so unfortunately ethPM v1 packages are no longer supported by the majority of ethPM tooling.

#### What about my IPFS assets?

While package authors should take their own measures to ensure IPFS assets maintain availability, the Ethereum Foundation sponsors a server that will automatically scrape, back-up, and persist IPFS assets published to any ERC1319 compliant ethPM registry. 

#### What's the best way to contribute?

Glad you asked! The easiest way to contribute is to package up your smart contracts, and make them available to the community by releasing them on your registry. Otherwise, pull requests to any of the existing libraries or integration into new frameworks would be massive - and earn the eternal love & appreciation of the ethPM community. 

#### Have another question? Ask it on our [Gitter channel](https://gitter.im/ethpm/Lobby).

#### Additional Reading

* \*\*\*\*[**Getting started with web3.py and EthPM**](https://snake-charmers.ethereum.org/2019/01/24/ethpm.html)\*\*\*\*



