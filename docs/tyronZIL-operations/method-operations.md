# tyronZIL DID method operations

> code-name: tyronOperations

This section specifies how a [tyronZIL-js](to-do add link) client is used to perform the create, read, update and deactivate operations.

## Create

How to create a DID and its associated DID document.

to-do: explore

import CreateOperation from '../../lib/core/versions/latest/CreateOperation';

## Read

How to use a DID to request its DID document. This operation corresponds to the [DID resolution](./DID-resolution.md) process.

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