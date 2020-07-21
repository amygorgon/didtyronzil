# tyronZIL DID-operations

The tyronZIL DID client performs the following operations:

- [Create](#create)
- [Read](#read)
- [Recover](#recover)
- [Update](#update)
- [Deactivate](#create)

## Create

How to generate a DID and its associated DID document.

> Details at [tyronZIL DID-create](./CRUD/did-create.md)

## Read

How to use a DID to request its DID document. This operation corresponds to the [DID resolution](./CRUD/did-resolve.md) process.

## Update

How to update a DID document, including the cryptographic operations necessary to establish proof of control.

> Details at [tyronZIL DID-update](./CRUD/did-update.md)

## Recover

To fully reset your DID document while keeping the same DID.

> Details at [tyronZIL DID-recover](./CRUD/did-recover.md)

## Deactivate

To deactivate a DID, including the cryptographic operations necessary to establish proof of deactivation.

> Details at [tyronZIL DID-deactivate](./CRUD/did-deactivate.md)

## Sidetree operation type

Conforming with the Sidetree protocol, tyronZIL DID operations MUST have a type equal to one variant of the OperationType enum, defined as follows:

```ts
enum OperationType {
  Create = 'create',
  Recover = 'recover'
  Update = 'update',
  Deactivate = 'deactivate',
}
```
