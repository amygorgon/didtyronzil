# Models

[TyronZIL-js](https://github.com/julio-cabdu/tyronZIL-js) implements the following data structures from the Sidetree protocol:

## Verification method model

```js
interface VerificationMethodModel {
    id: string;
    type: string;
    publicKeyBase58: string;
}
```

## Public key model

```js
interface PublicKeyModel {
    id: string;
    type: string;
    publicKeyBase58: string;
    purpose: PublicKeyPurpose[];
}
```

The type defaults to "SchnorrSecp256k1VerificationKey2019".

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

The current version only supports 'General' and 'Auth' purposes.

## DID Service endpoint model

```js
interface DidServiceEndpointModel {
    id: string;
    type: string;
    endpoint: string;
}
```

## Document model

```js
interface DocumentModel {
    public_keys: PublicKeyModel[];
    service_endpoints?: DidServiceEndpointModel[];
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
    AddKeys = 'add-public-keys',
    RemoveKeys = 'remove-public-keys',
    AddServices = 'add-service-endpoints',
    RemoveServices = 'remove-service-endpoints',
    CustomAction = '-custom-action',
}
```
