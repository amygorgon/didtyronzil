# Sidetree protocol

Here you can learn about the Sidetree protocol terminology and the default parameters in tyronZIL's implementation.

## Sidetree DID operation

A Sidetree DID operation is a set of delta-based modifications that change the state of a DID document when applied. The maximum uncompressed operation size (MAX_OPERATION_SIZE) has a default parameter of 1 kb.

## Sidetree operation request

JWS formatted request sent to a client to include a Sidetree DID operation in a batch of operations.

## Sidetree transaction

A Sidetree transaction writes an Anchor string in a DLT transaction of the underlying ledger.

### tyronZIL transaction

A tyronZIL transaction is a Sidetree transaction on the Zilliqa blockchain platform.

### Anchor string

A Sidetree Anchor string is the string anchored to the ledger, composed of the CAS URI of an [Anchor file](#anchor-file) prefixed with the declared operation count. The maximum number of operations per batch (MAX_OPERATION_COUNT) has a default parameter of 10,000.

### Sidetree transaction number

The Sidetreee transaction number is a monotonically increasing number. Its order is deterministic and assigned to every transaction according to its position in the ledger time.

### Ledger time

The ledger time is the blockchain clock variable, used as a deterministic chronological reference.

## Anchor file

An Anchor file is a JSON document containing proving and index data for the create, recovery and deactivate operations, as well as a CAS URI for the associated Map file. This file is anchored to the Zilliqa blockchain through a [Sidetree transaction](#sidetree-transaction). The maximum size of a compressed Anchor file (MAX_ANCHOR_FILE_SIZE) has a default parameter of 1 MB.

## Map file

A Map file is a JSON document containing proving and index data for the DID-update operation, as well as a CAS URI for the associated Chunk file. The maximum size of a compressed Map file (MAX_MAP_FILE_SIZE) has a default parameter of 1 MB.

## Chunk file

A Chunk file is a JSON document containing all the operation delta objects corresponding to the set of DIDs specified in the Map file. The maximum size of a compressed Chunk file (MAX_CHUNK_FILE_SIZE) has a default parameter of 10 MB.

## CAS

CAS stands for content-addressable storage. The CAS protocol utilized by tyronZIL is [IPFS](https://ipfs.io/).

### CAS URI

A CAS URI is a unique content-bound identifier used to locate a specific resource via the CAS protocol. The default CAS_URI_ALGORITHM to generate the CAS URI is IPFS CID.

## DID suffix

A DID suffix is the unique identifier string in a Decentralized Identifier, the last part of the DID after the final colon.

## Hash algorithm

The HASH_ALGORITH is the algorithm to generate hashes of protocol-related values. The default parameter is SHA256.

## Hash protocol

The HASH_PROTOCOL is the protocol to generate hash representations using the HASH_ALGORITHM. The default parameter is [Multihash](https://multiformats.io/multihash/): a protocol for differentiating outputs from various cryptographic hash functions, addressing size and encoding considerations.

## Data encoding scheme

The DATA_ENCODING_SCHEME is the encoding for various data structures such as JSON and hashes, which MUST have its output in ASCII format. The default parameter is Base64URL.

## Key algorithm

The KEY_ALGORITHM is the asymmetric public key algorithm to sign DID operations, which MUST be a valid JWK "crv". The default parameter is secp256k1.

## Operation key pair

Generates a cryptographic key pair to operate with, using the KEY_ALGORITHM. It returns the public key as a [PublicKeyModel](./implementation/models.md#public-key-model) and the private key as a secp256k1 key of type [JwkEs256k](./implementation/models.md#jwkes256k).

## Public key commitment

It is the resulting commitment obtained by applying the defined commitment scheme to a public key.

- **Update-commitment**:  
The resulting commitment obtained by applying the defined commitment scheme to the public key of an update key pair.

- **Recovery-commitment**:  
The resulting commitment obtained by applying the defined commitment scheme to the public key of a recovery key pair.

### Commitment scheme

A cryptographic primitive that commits to a chosen value, known as the commit value resulting in the generation of a commitment. Then, that commitment written in the DID document acts as a proof-of-commitment, without revealing the secret. The possessor of the corresponding private key can later use the commit value, to prove the right signature over the original commitment.

### Public key commitment scheme

Commitment scheme steps to generate a public key commitment from a public key:

1. Encode the public key into the form of a valid JWK
2. Canonicalize the JWK encoded public key using the JSON Canonicalization Scheme
3. Apply the defined HASH_ALGORITHM to the canonicalized public key to produce the public key commitment

Implementers MUST NOT re-use public keys across different commitment invocations.

## Signature algorithm

The SIGNATURE_ALGORITHM is the asymmetric public key signature algorithm, which MUST be a valid JWS "alg". The default parameter is ES256K.

## Commitment hash

The COMMITMENT_HASH is a cryptographically random hash of a value to be revealed in the next operation. The default parameter is 32 bytes.

## DID state patch

A DID state patch is the Sidetree format to describe the mutations of the DID's metadata state. Its data structure corresponds with the [Patch model](./implementation/models.md#patch-model), which MUST include a [Patch action](./implementation/models.md#patch-action) and the [document](./implementation/models.md#document-model) to be patched.

## Sidetree verification relationships

As defined in [W3C verification relationship](./W3C-dids.md#verification-relationship), the Sidetree protocol requires the following verification relationships:

```js
enum SidetreeVerificationRelationship {
    Operation = 'operation',
    Recovery = 'recovery'
}
```

- Operation: This verification relationship corresponds to a verification method of type update key, the key used to create the [update-commitment](#public-key-commitment), required by the [DID-update operation](../operations/CRUD/did-update.md).

- Recovery: This verification relationship corresponds to a verification method of type recovery key, the key used to create the [recovery-commitment](#public-key-commitment), required by the operations [DID-recover](../operations/CRUD/did-recover.md) and [DID-deactivate](../operations/CRUD/did-deactivate.md).
