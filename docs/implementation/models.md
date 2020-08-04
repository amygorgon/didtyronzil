# tyronZIL-js Sidetree models

**tyronZIL-js** is the open-source reference implementation for Node.js, written in TypeScript.

The Sidetree protocol requires the following data structures:

## JwkEs256k

Model to represent a secp256k1 key in a JWK format:

```js
interface JwkEs256k {
  kty: string;
  crv: string;
  x: string;
  y: string;
  kid?: string;
  d?: string; // Only used by a private key
}
```

## Verification method model

```js
interface VerificationMethodModel {
    id: string;
    type: string;
    controller?: string;
    publicKeyJwk: JwkEs256k;
}
```

## Public key model

```js
interface PublicKeyModel {
    id: string;
    type: string;
    controller?: string;
    jwk: JwkEs256k;
    purpose: PublicKeyPurpose[];
}
```

The type defaults to 'EcdsaSecp256k1VerificationKey2019'.

### Public key purpose

```js
enum PublicKeyPurpose {
  General = 'general',
  Auth = 'auth',
  Agreement = 'agreement',
  Assertion = 'assertion',
  Delegation = 'delegation',
  Invocation = 'invocation'
}
```

## Service endpoint model

```js
interface ServiceEndpointModel {
    id: string;
    type: string;
    endpoint: string;
}
```

## Document model

```js
interface DocumentModel {
    public_keys: PublicKeyModel[];
    service_endpoints?: ServiceEndpointModel[];
}
```

## Patch model

```js
interface PatchModel {
    action: PatchAction;
    publicKeys?: PublicKeyModel[];
    serviceEndpoints?: ServiceEndpointModel[];
    ids?: string[];
    document: DocumentModel;
}
```

### Patch action

```js
enum PatchAction {
    Replace = 'replace',
    AddKeys = 'add-public-keys',
    RemoveKeys = 'remove-public-keys',
    AddServices = 'add-service-endpoints',
    RemoveServices = 'remove-service-endpoints',
    CustomAction = '-custom-action',
}
```

Replace acts as a complete state reset that replaces a DID's current PKI metadata with the state provided - also used to create new DIDs.

## Delta model

```js
interface DeltaModel {
    patches: PatchModel[];
    update_commitment: string;
}
```

## Suffix data model

```js
interface SuffixDataModel {
    /** Encoded representation of the Create Operation Delta Object */
    delta_hash: string;
    recovery_commitment: string;
}
```

## Signed data models

Define the model for the JWS payload object required by the Update, Recover and Deactivate Operation Signed Data Object, respectively.

### Update

```js
interface UpdateSignedDataModel {
    // Encoded representation of the Update Operation Delta Object hash
    delta_hash: string;
    
    // The JCS canonicalized IETF RFC 7517 compliant JWK representation matching the previous update commitment value
    update_key: JwkEs256k;
}
```

### Recover

```js
export interface RecoverSignedDataModel {
    // Encoded representation of the Recovery Operation Delta Object hash
    delta_hash: string;
    
    // The JCS canonicalized IETF RFC 7517 compliant JWK representation matching the previous recovery commitment value
    recovery_key: JwkEs256k;
    
    // A new recovery commitment for the next recover operation
    recovery_commitment: string;
}
```

### Deactivate

```js
export interface DeactivateSignedDataModel {
    // The unique suffix of the DID to deactivate
    did_suffix: string;

    // The JCS canonicalized IETF RFC 7517 compliant JWK representation matching the previous recovery commitment value
    recovery_key: JwkEs256k;
}
```

## DID state

```js
interface DidStateModel {
    document: DocumentModel;
    updateCommitment: string | undefined;
    recoverCommitment: string | undefined;
    lastTransactionNumber?: number;
}
```

The commitments are ```undefined``` after the deactivate operation.

## Anchored operation model

```js
interface AnchoredOperationModel {
  // The original request buffer sent by the requester
  operationBuffer: Buffer;
  
  // The DID unique suffix
  didUniqueSuffix: string;
  
  // The type of operation
  type: OperationType;
  
  // The logical blockchain time that this operation was anchored on the blockchain
  ledgerTime: number;
  
  // The transaction number of the transaction this operation was batched within
  transactionNumber: number;
  
  // The index this operation was assigned to in the batch
  operationIndex: number;
}
```
