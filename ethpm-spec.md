# ethPM Specification

The [ethPM-Spec](http://ethpm.github.io/ethpm-spec/package-spec.html) defines the JSON schema for an ethPM manifest. ethPM manifests are only valid if they are tightly packed, with alphabetically sorted keys, and no trailing whitespaces. Since ethPM relies heavily upon content-addressed URIs, it's crucial that ethPM manifests are properly formatted, since the smallest edit will completely change the content's hash and integrity.

The formal definition of the ethPM-Spec can be found at its [documentation](http://ethpm.github.io/ethpm-spec/). Below is a brief overview of the different fields you can define in an ethPM manifest to capture the idea of your smart contract\(s\).

#### "package\_name" \(required\)

The name of your ethPM package. Must conform to regex: `^[a-zA-Z][a-zA-Z0-9_]{0,255}$`

* Valid package names
  * `wallet`
  * `token-20`
  * `dao_123`
* Invalid package names
  * `1token`
  * `_mypackage`
  * `wallet.123`

#### "version" \(required\)

The version of your ethPM package. ethPM does not enforce a specific versioning scheme, but using [semver](https://semver.org/) is strongly encouraged. 

* `"1.0.0"`
* `"2.0.0b3"`
* `"09.07.2019"`

#### "manifest\_version" \(required\)

The ethPM version of your manifest. Currently, ethPM tooling only supports v2 of the ethPM spec.

* `"2"`

#### "meta"

JSON object containing metadata for your package. Possible options include..

* `"authors": ["Alice", "Bob"]`
* `"license": "MIT"`
* `"description": "My awesome package."`
* `"keywords": ["solidity", "ethPM", "wallet"]`
* `"links": {`

      `"documentation": "readthedocs.com",`

      `"repository": "github.com",`

      `"website": "wallet.com"`

  `}`

#### "contract\_types"

A field containing all of the contract types defined in a manifest. A contract type can be thought of as the fundamental unit of an ethPM package. Two contracts are of the same contract type if they have the same bytecode. The `"contract_types"` field contains the following properties for a defined type.

* `"contract_name"`
* `"deployment_bytecode"`
* `"runtime_bytecode"`
* `"abi"`
* `"natspec"`
* `"compiler"`

#### "sources"

This field contains content-addressed URIs for smart contracts composing the contract types defined in a manifest. Keys must be relative filesystem paths beginning with a `./`. Values can be either the entire source contract, inlined as a single string `or` a content-addressed URI where the source contract can be found.

While the `sources` field must contain all smart contracts necessary to compile the defined `contract_types` in the manifest, the `sources` field is not limited to just smart contracts, and can contain deployment scripts or other files that would be useful to interact with the package's smart contract idea.

#### "deployments"

This field contains the deployment data for instances of deployed contract types. This field can contain references to contract deployments on different blockchains. [Blockchain URIs](uris.md#blockchain-uris) are used to distinguish between the different blockchain networks. For each deployment, the following fields are available.

* `"contract_type"` \(required\)
* `"address"` \(required\)
* `"transaction"`
* `"block"`
* `"runtime_bytecode"`
* `"compiler"`

#### "build\_dependencies"

Content-addressed URIs for any ethPM packages that a manifest depends upon.

