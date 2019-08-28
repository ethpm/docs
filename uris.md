# URIs

ethPM uses a variety of URI schemes to help identify specific resources.

#### Content-Addressed URIs

Content-addressed URIs are used to ensure the immutability of each package release. A content-addressed URI is any URI that contains a cryptographic hash ensuring the identity of its contents and that the contents have not been modified. For example...

* [IPFS](https://ipfs.io/)
  * `ipfs://Qme4otpS88NV8yQi8TfTP89EsQC5bko3F5N1yhRoi6cwGV`
* [Swarm](https://swarm-guide.readthedocs.io/en/latest/introduction.html)
  * `bzz://2477cc8584cc61091b5cc084cdcdb45bf3c6210c263b0143f030cf7d750e894d`
* [Git Blobs](https://developer.github.com/v3/git/blobs/)
  * `https://api.github.com/repos/:owner/:repo/git/blobs/:file_sha`

#### Registry URIs

A registry URI is used to specify an on-chain registry or a specific release on a registry.

`erc1319://[CONTRACT_ADDRESS]:[CHAIN_ID]`

* ex: `erc1319://0x6b5DA3cA4286Baa7fBaf64EEEE1834C7d430B729:1`

`erc1319://[CONTRACT_ADDRESS]:[CHAIN_ID]/[PACKAGE_NAME]?version=[PACKAGE_VERSION]`

* ex:`erc1319://0x6b5DA3cA4286Baa7fBaf64EEEE1834C7d430B729:1/owned?version=1.0.0`

#### Blockchain URIs

A blockchain URI is used to specify the blockchain on which a deployed contract instance lives. This definition originates from [BIP122 URI](https://github.com/bitcoin/bips/blob/master/bip-0122.mediawiki).

* `blockchain://[CHAIN_ID]/block/[BLOCK_HASH]`
  * `CHAIN_ID` is the unprefixed hexadecimal representation of the genesis hash for the chain.
  * `BLOCK_HASH` is the unprefixed hexadecimal representation of the hash of a block on the chain.

#### Etherscan URIs

`ethPM CLI` uses this URI scheme to automatically generate packages for any of Etherscan's verified contracts. The provided contract address and chain ID **must** correspond to a contract address that has been verified \(hint. look for the green checkmark in the `Contract` tab\).

`etherscan://[CONTRACT_ADDRESS]:[CHAIN_ID]`

* ex: `etherscan://0xdAC17F958D2ee523a2206206994597C13D831ec7:1`

