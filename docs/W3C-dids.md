# W3C Decentralized Identifiers

The tyronZIL DID Method refers to the following terms specified in [W3C Decentralized Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/):

## DID Method

A DID Method is a specification for the precise scheme of a DID, and it also specifies the methods creating, resolving, updating and deactivating such a DID and its DID Document, using a specific type of verifiable data registry, in tyronZIL's case, Zilliqa.

## Decentralized Identifier (DID)

A DID is a globally unique Uniform Resource Identifier (URI) that associates a DID Subject with a DID Document. Given the decentralized nature of Zilliqa, a tyronZIL DID has its existence guaranteed without depending on a central authority.

A DID consists of three parts:

- The scheme identifier: "did"
- The DID Method identifier
- The DID Method-specific identifier, which must be unique

> The Tyron [DID Scheme](./scheme/did-scheme.md).


### DID URL

A DID URL identifies a particular resource to be located, e.g. a specific part of the DID Document. It extends the syntax of a basic DID to incorporate other standard URI components:

- Path: The portion of a DID URL that begins with and includes the first forward slash character, "/"
- Query: The portion of a DID URL that follows the first question mark character, "?"
- Fragment: The portion of a DID URL that follows the first hash sign character "#"

## DID Subject

The DID Subject is the entity identified by the DID and described by the DID Document - the user of the Decentralized Identifier.

### DID Controller

The DID Controller is the entity that has the capability - as defined by the DID Method - to make changes to a DID Document.

## DID Document

A DID is resolvable to a DID Document, which contains data associated with the DID, such as cryptographic material, verification methods and service endpoints relevant to interactions with the DID Subject. The DID itself is the value of the ***id*** property.

> The Tyron [DID Document](./did-document.md).

## Verification method

A verification method is a set of parameters used to independently verify a proof according to the particular DID Method, e.g. a public key.

### Verification relationship

It expresses the relationship between the DID Subject and a verification method, e.g. authentication. All verification methods must be associated with a particular verification relationship.

## Producer

A producer is any algorithm realized as software/hardware that conforms to the W3C DIDs specification by generating conforming DIDs or conforming DID documents. A producer that is conformant with the specification MUST NOT produce non-conforming DIDs or DID documents. Producers MUST indicate which representation of a document corresponds via a 'contentType' in the document's resolution-metadata.

## Consumer

A consumer is any algorithm realized as software/hardware that conforms to the W3C DIDs specification by consuming conforming DIDs or conforming DID documents. A consumer that is conformant with the specification MUST produce errors when consuming non-conforming DIDs or DID documents. Consumers MUST determine which is the representation of the DID Document via the 'contentType' DID resolver metadata field.
