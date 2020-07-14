# tyronZIL DID operations

TyronZIL DID operations are: to-do

> code-name:

This section specifies how the [tyronZIL-js client](https://github.com/julio-cabdu/tyronZIL-js) is used to perform the create, read, update and deactivate operations.

## Create

> code-name: DidCreate

How to create a DID and its associated DID document.

to-do: explore

import CreateOperation from '../../lib/core/versions/latest/CreateOperation';

## Read

How to use a DID to request its DID document. This operation corresponds to the [DID resolution](./CRUD/did-resolution.md) process.

## Update

How to update a DID document, including the cryptographic operations necessary to establish proof of control.

to-do specify what properties can be updated

## Deactivate

How to deactivate a DID, including the cryptographic operations necessary to establish proof of deactivation.

## Sidetree OperationType

The Sidetree OperationType MUST be an enum defined as follows:

```js
enum OperationType {
  Create = 'create',
  Update = 'update',
  Deactivate = 'deactivate',
  Recover = 'recover'
}
```

> Defined in _sidetree/lib/core/enums/OperationType.ts_.