# DID resolution

It corresponds to the READ operation of a DID. The DID resolution function resolves a DID into its DID document by performing the process called DID resolution.

A DID resolver is the software component that executes the DID resolution function. It takes a DID (and metadata) as input and produces a conforming DID document (and metadata) as output.

## DID URL dereferencing

DID URL dereferencing is the process that returns the particular resource specified by the DID URL. It can use the DID resolution function to fetch the DID document and then it performs additional processing on the DID document to return the dereferenced resource that was requested.

The software component is called DID URL dereferencer, and it takes as input a DID URL, a DID document and a set of dereferencing options and returns the specific resource. The dereferencing options control how the resource is dereferenced.

## DID resolution variables

### Input variables

- **did**: A conformant DID as a single string. This is the DID to resolve. This input is REQUIRED.
- **did-resolution-input-metadata**: A metadata structure consisting of input options to the resolve function in addition to the DID itself. This input is REQUIRED, but the structure MAY be empty. The options control how the DID is resolved.

### Output variables

- **did-resolution-metadata**: A metadata structure consisting of values relating to the results of the DID resolution process. This structure is REQUIRED and MUST NOT be empty. This metadata typically changes between invocations of the resolve function as it represents data about the resolution process itself.
- **did-document-stream**: This MUST be a byte stream of the resolved DID document in one of the conformant representations. The byte stream MAY then be parsed by the caller of the resolve function into a DID document abstract data model, which can in turn be validated and processed. If the resolution is unsuccessful, this value MUST be an empty stream.
- **did-document-metadata**: This MUST be a metadata structure. This structure contains metadata about the DID document contained in the did-document-stream. This metadata typically does not change between invocations of the resolve function unless the DID document changes, as it represents data about the DID document. If the resolution is unsuccessful, this output MUST be an empty metadata structure.