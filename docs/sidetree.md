# Sidetree protocol

Here you can learn about the Sidetree protocol terminology and the default parameters used by tyronZIL.

## Sidetree DID operation

A Sidetree DID operation is a set of delta-based modifications that change the state of a DID-Document when applied.

### Sidetree transaction number

The Sidetreee transaction number is a monotonically increasing number. Its order is deterministic and assigned to every transaction according to its position in the ledger time.

In tyronZIL's implementation, each DID has a corresponding Tyron-Smart-Contract that keeps track of the DID-State and assigns a Sidetree transaction number to every consecutive transaction modifying the DID-State.

### Ledger time

The ledger time is the blockchain clock variable, used as a deterministic chronological reference.

## DID-Suffix

A DID-Suffix is the unique identifier string in a Decentralized Identifier, the last part of the DID after the final colon.

## Long-Form DID-URI

This type of [DID-URL](./W3C-dids.md#did-url) has the necessary information to immediately use a DID without anchoring it to the underlying ledger (therefore, the DID is unpropagated/unpublished).

Format:  
```DID?-sidetree-initial-state=<create-operation-suffix-data-object>.<create-operation-delta-object>```

More info [here](./scheme/did-url-syntax.md#sidetree-long-form-did)

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

A cryptographic primitive that commits to a chosen value, known as the commit value resulting in the generation of a commitment. Then, that commitment written in the DID-Resolution-Result acts as a proof-of-commitment, without revealing the secret. The possessor of the corresponding private key can later produce the commit value, proving its validity against the original commitment.

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

## DID-State patch

A DID-State patch is the Sidetree format to describe the mutations of the DID's metadata state. Its data structure corresponds with the [Patch model](./implementation/models.md#patch-model), which MUST include a [Patch action](./implementation/models.md#patch-action) and the [document](./implementation/models.md#document-model) to be patched.

## Sidetree verification relationships

As defined by the [W3C verification relationship](./W3C-dids.md#verification-relationship), the Sidetree protocol requires the following verification relationships:

```js
enum SidetreeVerificationRelationship {
    Operation = 'operation',
    Recovery = 'recovery'
}
```

- Operation: This verification relationship corresponds to a verification method of type update key, the key used to create the [update-commitment](#public-key-commitment), required by the [DID-Update operation](./operations/CRUD/did-update.md).

- Recovery: This verification relationship corresponds to a verification method of type recovery key, the key used to create the [recovery-commitment](#public-key-commitment), required by the operations [DID-Recover](./operations/CRUD/did-recover.md) and [DID-Deactivate](./operations/CRUD/did-deactivate.md).
