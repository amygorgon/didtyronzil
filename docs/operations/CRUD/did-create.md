# tyronZIL DID-create operation

A tyronZIL DID-create operation is conformant with a Sidetree Create operation, and its goal is to generate a brand new tyronZIL Decentralized Identifier.

Follow these steps:

## 1. Generate the verification methods

1.1 publicKeys: [PublicKeyModel[]](../../implementation/models.md#public-key-model)     -->     using the [operation key pair](../../sidetree.md#operation-key-pair)

1.2 operation: [Operation](../../implementation/models.md#sidetree-verification-methods)

1.3 recovery: [Recovery](../../implementation/models.md#sidetree-verification-methods)

## 2. Generate the [public key commitments](../../sidetree.md#public-key-commitment)

2.1 Use the UPDATE_KEY to generate the update-commitment:

UPDATE_KEY = operation.publicKeyJwk

UPDATE_COMMITMENT = [Multihash](../../sidetree.md#hash-protocol).canonicalize[ThenHash](../../sidetree.md#commitment-hash)[ThenEncode](../../sidetree.md#data-encoding-scheme)(UPDATE_KEY)

2.2 Use the RECOVERY_KEY to generate the recovery-commitment:

RECOVERY_KEY = recovery.publicKeyJwk

RECOVERY_COMMITMENT = [Multihash](../../sidetree.md#hash-protocol).canonicalize[ThenHash](../../sidetree.md#commitment-hash)[ThenEncode](../../sidetree.md#data-encoding-scheme)(RECOVERY_KEY)

## 3. Generate the service endpoints

service: [ServiceEndpointModel[]](../../implementation/models.md#service-endpoint-model)

## 4. Generate the document model

DOCUMENT: [DocumentModel](../../implementation/models.md#document-model) = {  
public_keys: publicKeys,  
service_endpoints: service  
}

## 5. Generate the patch

For the DID-create operation, the patch action is 'replace'.

PATCH: [PatchModel](../../implementation/models.md#patch-model) = {  
action: [PatchAction](../../implementation/models.md#patch-action).Replace,  
document: DOCUMENT  
}

## 6. Generate the Create Operation Delta Object

DELTA: [DeltaModel](../../implementation/models.md#delta-model) = {  
patches: [PATCH],  
updateCommitment: UPDATE_COMMITMENT  
}

Then apply the following operations to the object:

6.1 Stringify  
6.2 Turn it into a buffer  
6.3 Encode it with the [data encoding scheme](../../sidetree.md#data-encoding-scheme)  
6.4 Hash it with the [hash algorithm](../../sidetree.md#hash-algorithm) and the [hash protocol](../../sidetree.md#hash-protocol)  
6.4 Encode it again and call it DELTA_HASH  

### DID suffix

The did-suffix MUST be globally unique and generated as follows, in conformance with the Sidetree protocol:

1. Generate verification-methods/signing-key-pairs using the [operationKeyPair](./sidetree/sidetree.md#operation-key-pair):

    - The public keys are of type [PublicKeyModel](./sidetree/models.md#public-key-model)
    - The private keys have type [JwkEs256k](./sidetree/models.md#jwkes256k)

2. Generate service endpoints of type [ServiceEndpointModel](./sidetree/models.md#service-endpoint-model)

3. Create a new document of type [DocumentModel](./sidetree/models.md#service-endpoint-model) as follows:

        const DOCUMENT: DocumentModel {
            publicKeys: [PublicKeyModel],
            service_endpoints: [ServiceEndpointModel]
        }

4. Put the document inside of a [PatchModel](./sidetree/models.md#patch-model), with a 'replace' [PatchAction](./sidetree/models.md#patch-action):

        const PATCH: PatchModel {
            action: PatchAction.Replace,
            document: DOCUMENT
        }

5. Generate an update key-pair of type [JwkEs256k](./sidetree/models.md#jwkes256k) and use its public key for the [update-commitment](./sidetree/sidetree.md#public-key-commitment).

6. Produce the Create Operation Delta Object of type [DeltaModel](./sidetree/models.md#delta-model) using the patch and the update-commitment:


