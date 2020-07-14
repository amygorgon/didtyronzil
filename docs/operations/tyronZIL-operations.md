# tyronZIL DID operations

The tyronZIL DID client performs the following operations:

- [Create](#create)
- [Resolve](#resolve)
- [Recover](#recover)
- [Update](#update)
- [Deactivate](#create)

## Create

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