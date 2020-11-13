# Models

[TyronZIL-js](https://github.com/julio-cabdu/tyronZIL-js) extends the implementation of the following data structures from the [Sidetree protocol](https://identity.foundation/sidetree/spec/):

## Public key model

```js
interface PublicKeyModel {
    id: string;
    key?: string;
}
```
The ID MUST be unique and correspond to one of the Public Key Purposes.

## Verification method model

It extends the public key model:

```js
interface VerificationMethodModel {
    id: string;
    type: string;
    publicKeyBase58: string;
}
```
The type defaults to "SchnorrSecp256k1VerificationKey2019".

### Public key purpose

It corresponds to the [Verification Relationship](../W3C-dids.md#verification-relationship) for a public key aka verification method.

```js
enum PublicKeyPurpose {
  General = 'general',
  Auth = 'auth',
  Agreement = 'agreement',
  Assertion = 'assertion',
  Delegation = 'delegation',
  Invocation = 'invocation',
  XSGD = 'xsgd'
}
```

## DID Service endpoint model

```js
interface DidServiceEndpointModel {
    id: string;
    type: string;
    endpoint: string;
}
```

## Patch model

```js
interface PatchModel {
    action: PatchAction;
    ids?: string[];        //the IDs of the DID-Document elements to remove
    keyInput?: PublicKeyInput[];
    services?: TransitionValue[];
}
```

### Patch action

```js
enum PatchAction {
    AddKeys = 'add-public-keys',
    RemoveKeys = 'remove-public-keys',
    AddServices = 'add-service-endpoints',
    RemoveServices = 'remove-service-endpoints',
    CustomAction = '-custom-action',
}
```
