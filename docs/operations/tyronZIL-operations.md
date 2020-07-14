# tyronZIL DID operations

The tyronZIL DID client performs the following operations:

- [Create](#create)
- [Read](#resolve)
- [Recover](#recover)
- [Update](#update)
- [Deactivate](#create)

## Create

How to create a DID and its associated DID document.

## Read

How to use a DID to request its DID document. This operation corresponds to the [DID resolution](./CRUD/did-resolve.md) process.

## Update

How to update a DID document, including the cryptographic operations necessary to establish proof of control.

## Deactivate

How to deactivate a DID, including the cryptographic operations necessary to establish proof of deactivation.

## Sidetree operation type

In compliance with the Sidetree protocol, tyronZIL DID operations MUST have a type equal to one variant of the OperationType enum, defined as follows:

```ts
enum OperationType {
  Create = 'create',
  Recover = 'recover'
  Update = 'update',
  Deactivate = 'deactivate',
}
```

> Defined in _sidetree/lib/core/enums/OperationType.ts_
