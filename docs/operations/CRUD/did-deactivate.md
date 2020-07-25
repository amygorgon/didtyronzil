# tyronZIL DID-deactivate operation

A tyronZIL DID-deactivate operation is conformant with a Sidetree Deactivate operation, and its goal is to deactivate the given tyronZIL DID and set its DID-state to 'deactivate'.

Follow these steps:

## 0. User input

Get the DID to deactivate and the recovery private key.

## 1. The Deactivate Operation Signed Data Object

1.1 Use the recovery private key to get the ```RECOVERY_KEY``` from the previous recovery operation.Then generate an instance of the Deactivate Operation Signed Data Object as follows:

```js
SIGNED_DATA = {  
    did_suffix: did_tyronZIL.didUniqueSuffix,
    recovery_key: PREVIOUS_RECOVERY_KEY  
}
```

> ```SIGNED_DATA``` is of type [DeactivateSignedDataModel](../../implementation/models.md#deactivate)

1.2 Then sign this object as a e256k compact JWS, signing it with the private key from the previous recovery operation (user input).

```ts
const SIGNED_DATA_JWS = await Cryptography.signUsingEs256k(SIGNED_DATA, recoveryPrivateKey);
```

## 2. Sidetree request

2.1 Generate the following object:

```js
SIDETREE_REQUEST = {  
    did_suffix: did_tyronZIL.didUniqueSuffix,
    signed_data: SIGNED_DATA_JWS,
    type: OperationType.deactivate
}
```

1.2 Stringify it and turn it into a buffer as ```OPERATION_BUFFER```  
2.3 Send the ```OPERATION_BUFFER``` to the Sidetree's library DeactivateOperation, which returns a class with the following properties:

- The original request buffer sent by the requester:

```operationBuffer: Buffer```

- The unique suffix of the DID, globally unique:

```didUniqueSuffix: string```

- The data used to generate the Deactivate Operation Signed Data Object:

```signedDataModel: DeactivateSignedDataModel```

- The compact JWS signature that deactivated the DID

```signedDataJws: Jws```

- The deactivate Operation Delta Object:

## 3. tyronZIL DID-deactivate operation result

The return value of a tyronZIL-js DID-deactivate operation is an instance of the class [DidDeactivate](https://github.com/julio-cabdu/tyronZIL-js/tree/master/src/lib/did-operations/did-deactivate.ts), which includes all the previously mentioned properties plus the DID in format tyronZIL DID-scheme.

After deactivation, the DID cannot be utilized again and its DID-state will always in status 'deactivate'.

```type: OperationType.Deactivate```
