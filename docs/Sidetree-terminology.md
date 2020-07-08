# Sidetree protocol terminology and default parameters in tyronZIL

## Anchor file

An Anchor file is a JSON document containing proving and index data for the create, recovery and deactivate operations, as well as a CAS URI for the associated Map file. This file is anchored to the Zilliqa blockchain through a [Sidetree transaction](#Sidetree-transaction). The maximum size of a compressed Anchor file (MAX_ANCHOR_FILE_SIZE) has a default parameter of 1 MB.

## Map file

A Map file is a JSON document containing proving and index data for the Update operation, as well as a CAS URI for the associated Chunk file. The maximum size of a compressed Map file (MAX_MAP_FILE_SIZE) has a default parameter of 1 MB.

## Chunk file

A Chunk file is a JSON document containing all verbose operation data for the corresponding set of DIDs specified in the corresponding Map file. The maximum size of a compressed Chunk file (MAX_CHUNK_FILE_SIZE) has a default parameter of 10 MB.

## CAS

CAS stands for content-addressable storage. The CAS protocol utilized by tyronZIL is IPFS.

### CAS URI

- A CAS URI is a unique content-bound identifier used to locate a specific resource via the CAS protocol. The default CAS_URI_ALGORITHM to generate the CAS URI is IPFS CID.

## DID suffix

A DID suffix is the unique identifier string in a Decentralized Identifier, the last part of the DID after the final colon.

## Sidetree operation request

A Sidetree operation request is a wweration in a batch of operations.

## Sidetree DID operation

A Sidetree DID operation is a set of delta-based modifications that change the state of a DID document when applied. The maximum uncompressed operation size (MAX_OPERATION_SIZE) has a default parameter of 1 kb.

## Sidetree transaction

A Sidetree transaction writes an Anchor string in a DLT transaction of the underlying ledger.

### Anchor string

- An Anchor string is the string anchored to the ledger, composed of the CAS URI of an Anchor file prefixed with the declared operation count. The maximum number of operations per batch (MAX_OPERATION_COUNT) has a default parameter of 10,000.

### Sidetree transaction number

- The Sidetreee transaction number is a monotonically increasing number deterministically ordered and assigned to every transaction relative to its position in the ledger time.

### Ledger time

> code-name: ledgerTime

- The ledger time is the deterministic clock variable of the underlying blockchain that is used as a deterministic chronological reference.

## Hash algorithm

The HASH_ALGORITH is the algorithm to generate hashes of protocol-related values. The default parameter is SHA256.

## Hash protocol

The HASH_PROTOCOL is the protocol to generate hash representations using the HASH_ALGORITHM. The default parameter is [Multihash](https://multiformats.io/multihash/), a protocol for differentiating outputs from common cryptographic hash functions, addressing size and encoding considerations.

## Data encoding scheme

The DATA_ENCODING_SCHEME is the encoding for various data structures such as JSON and hashes, which MUST have its output in ASCII format. The default parameter is Base64URL.

## Key algorithm

> code-name: keyAlgorithm

The KEY_ALGORITHM is the asymmetric public key algorithm to sign DID operations, which MUST be a valid JWK "crv". The default parameter is secp256k1.

## Signature algorithm

The SIGNATURE_ALGORITHM is the asymmetric public key signature algorithm, which MUST be a valid JWS "alg". The default parameter is ES256K.

## Commitment hash

The COMMITMENT_HASH is a cryptographically random hash of a value to be revealed in the next operation. The default parameter is 32 bytes.

## Reveal value

The REVEAL_VALUE is a cryptographically random value to be revealed in the next operation. The default parameter is 100 bytes.

to-do dive into MAX_ENCODED_HASH_LENGHT and GENESIS_TIME
