# tyronZIL DID operations

TyronZIL implements Sidetree delta-based operations to generate the DID-Suffix and the DID-Document as well as any following update, recover or deactivation by utilizing Sidetree public-key commitments to establish proof-of-control.

Once the Sidetree operation request is validated, the DID-Client MUST submit the DID-State modifications to the user's Tyron-Smart-Contract, which is deployed by the DID-Create operation.

The tyronZIL DID-Client performs the following operations:

- [Create](./CRUD/did-create.md): How to generate a DID and its associated DID-Document. Plus how to save the DID on Zilliqa by deploying a Scilla smart-contract.
- [Read](./CRUD/did-resolve.md): How to use a DID to request its DID-Document. This operation corresponds to the DID-Resolution process.
- [Recover](./CRUD/did-recover.md): To fully reset your DID-Document while keeping the same DID.
- [Update](./CRUD/did-recover.md): How to update verification methods/ service endpoints in the DID-Document.
- [Deactivate](./CRUD/did-deactivate.md): How to deactivate a DID, forever. It turns all DID-State variables to undefined.

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
