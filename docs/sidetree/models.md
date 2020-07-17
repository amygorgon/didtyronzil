# Sidetree models

The Sidetree protocol requires the following data structures, which are in TypeScript for the tyronZIL-js implementation:

## Public key model

```js
interface PublicKeyModel {
    id: string;
    type: string;
    jwk: JwkEs256k;
    purpose: PublicKeyPurpose[];
}
```

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
    publicKeys?: PublicKeyModel[];
    serviceEndpoints: ServiceEndpointModel[];
}
```

## Patch model

```js
interface PatchModel {
    action: PatchAction;
    publicKeys?: PublicKeyModel[]; // if action = RemoveKeys => publicKeys = PublicKeyModel .. to-do
    serviceEndpoints?: ServiceEndpointModel[];
    ids?: string[];
    document: DocumentModel;
}
```

### Patch action

```js
enum PatchAction {
    AddKeys = 'add-public-keys',
    RemoveKeys = 'remove-public-keys',
    AddServices = 'add-service-endpoints',
    RemoveServices = 'remove-service-endpoints',
    Replace = 'replace',
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

## Suffix Data Model

```js
interface SuffixDataModel {
    delta_hash: string;
    recovery_commitment: string;
}
```

## DID state

```js
interface DidState {
    document: DocumentModel;
    nextRecoveryCommitmentHash: string | undefined;
    nextUpdateCommitmentHash: string | undefined;
    lastOperationTransactionNumber: number;
}
```

The commitments are undefined after the deactivate operation.

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
