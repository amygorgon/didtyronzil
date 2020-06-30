# W3C definitions in tyronZIL

## DID method

A DID method is the specification for the precise scheme of a DID, and it also specifies the methods creating, resolving, updating and deactivating such a DID and its DID document, using a specific type of verifiable data registry, in tyronZIL's case Zilliqa.

## Decentralized Identifier (DID)

A DID is a globally unique Uniform Resource Identifier (URI) that associates a DID subject with a DID document. Given the decentralized nature of Zilliqa, a tyronZIL DID has its existence guaranteed without depending on a central authority.

A DID consists of three parts:

- The scheme identifier: "did"
- The DID method identifier: "tyron:zil"
- The DID method-specific identifier, which must be unique

More information at [tyronZIL DID scheme](./DID-scheme.md).

### DID URL

A DID URL extends the syntax of a basic DID to incorporate other standard URI components:

- Path: The portion of a DID URL that begins with and includes the first forward slash character, "/".
- Query: The portion of a DID URL that follows the first question mark character, "?".
- Fragment: The portion of a DID URL that follows the first hash sign character "#".

## DID subject

The DID subject is the entity identified by the DID and described by the DID document.

### DID controller

The DID controller is the entity that has the capability - as defined by the DID method - to make changes to a DID document.

> tyronZIL DID subjects are also the controllers of their own DIDs.

## DID document

A DID is resolvable to a DID document, which contains metadata associated with the DID, such as cryptographic material, verification methods and service endpoints relevant to interactions with the DID subject. The DID itself is the value of the id property.

More information at [tyronZIL DID documents](./DID-document.md).
