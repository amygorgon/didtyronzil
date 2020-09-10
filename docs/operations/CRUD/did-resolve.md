# tyronZIL DID-Resolve operation

## DID-Resolution

It corresponds to the READ operation of a Decentralized Identifier. The DID-Resolution function resolves a DID into its DID-Document by performing the process called DID-Resolution.

A DID-Resolver is the software component that executes the DID-Resolution process. It takes a DID (and metadata) as input and produces a conforming DID-Document (and metadata) as output, which is called the DID-Resolution-Result.

## DID-Resolution variables for tyronZIL DIDs

### Input variables

- **did**: A conformant DID as a single string. This is the DID to resolve. This input is REQUIRED.
- **network**: The 'NetworkNamespace' referring to the testnet or the mainnet.
- **tyronAddr**: The Zilliqa address of the user's Tyron-Smart-Contract. In upcoming versions, the tyronAddr will get retrieved from a DNS-like smart-contract.
- **metadata**: The DID-Resolution input metadata is a structure consisting of input options to the resolve function in addition to the DID itself. This input is REQUIRED. The options control how the DID is resolved. TyronZIL-v0.4 only supports the 'Accept' option that defines if the result shall be the corresponding DID-Document with or without the Document-metadata. The former produces an output called DID-Resolution-Result.

### Output variables

- **resolutionMetadata**: A metadata structure consisting of values relating to the results of the DID-Resolution process. This structure is REQUIRED and MUST NOT be empty. This metadata typically changes between invocations of the resolve function as it represents data about the resolution process itself.
TyronZIL's 'resolutionMetadata' refers to the Zilliqa's 'GetBlockchainInfo' method that returns the network statistics for the specified network at the time of the request.
- **document**: This MUST be the resolved DID-Document serialized in JSON format.
- **documentMetadata**: This structure contains metadata about the Decentralized Identifier of the DID-Document. It typically does not change between invocations of the resolve method unless the DID-Document changes. TyronZIL's 'DocumentMetadata' includes three properties: the 'contentType' equal to "application/did+json", the 'updateCommitment' and the 'recoveryCommitment'.

> Find the tyronZIL DID-Resolution method [here](https://github.com/julio-cabdu/tyronZIL-js/blob/master/src/lib/decentralized-identity/did-document.ts)

#### Resolving a deactivated DID

If a tyronZIL DID status is 'deactivate' in its corresponding Tyron-Smart-Contract-State, then the DID-Resolver MUST throw a 'DidDeactivated' error.

## DID-URL dereferencing

DD-URL dereferencing is the process that returns the particular resource specified by the DID-URL. It can use the DID-Resolution process to fetch the DID-Document and then it performs additional processing on the DID-Document to return the dereferenced resource that was requested.

The software component is called DID-URL-Dereferencer, and it takes as input a DID-URL, a DID-Document and a set of dereferencing options and returns the specific resource. The dereferencing options control how the resource is dereferenced.

> DID-URL dereferencing will be supported in future versions of tyronZIL
