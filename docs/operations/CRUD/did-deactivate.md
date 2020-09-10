# tyronZIL DID-Deactivate operation

A tyronZIL DID-Deactivate operation is conformant with a Sidetree Deactivate operation, and its goal is to deactivate the given tyronZIL DID and set its DID-State to 'deactivate'.

Follow these steps:

## 0. User input

Ask the user for the recovery-private key corresponding to the previous recovery-commitment, as well as the address of the user's Tyron-Smart-Contract(TSM) to deactivate their DID. The client MUST fetch the TSM-State and check that the key given by the user corresponds to the recovery-commitment in the TSM-State.

## 1. Deactivate Operation Signed Data Object

1.1 Use the recovery private-key to get the ```RECOVERY_KEY``` corresponding to the previous recovery-commitment. Then generate an instance of the Deactivate Operation Signed Data Object as follows:

```js
SIGNED_DATA = {  
    did_suffix: did_tyronZIL,
    recovery_key: PREVIOUS_RECOVERY_KEY  
}
```

> ```SIGNED_DATA``` is of type [DeactivateSignedDataModel](../../implementation/models.md#deactivate)

1.2 Then sign this object as a e256k compact JWS, signing it with the private-key corresponding to the previous recovery-commitment.

```ts
const SIGNED_DATA_JWS = await Cryptography.signUsingEs256k(SIGNED_DATA, previousRecoveryPrivateKey);
```

## 2. Sidetree request

2.1 Generate the following object:

```js
SIDETREE_REQUEST = {  
    did_suffix: did_tyronZIL,
    signed_data: SIGNED_DATA_JWS,
    type: OperationType.deactivate
}
```

1.2 Stringify it and turn it into a buffer as ```SIDETREE_REQUEST_BUFFER```  
2.3 Process the ```SIDETREE_REQUEST_BUFFER``` with the Sidetree's library DeactivateOperation, which returns a class with the following properties:

- The original request buffer sent by the requester:

```operationBuffer: Buffer```

- The unique suffix of the DID, globally unique:

```didUniqueSuffix: string```

- The data used to generate the Deactivate Operation Signed Data Object:

```signedDataModel: DeactivateSignedDataModel```

- The compact JWS signature that deactivated the DID:

```signedDataJws: Jws```

## 3. Tyron-Smart-Contract(TSM)

Once the Sidetree Deactivate Operation has been successful, the client MUST submit a transaction that calls the 'DidDeactivate' transition of the user's TSM. The required parameter is the previous recovery-commitment. This transition sets the status field to 'deactivate' and all other DID-State fields to 'undefined'.

After deactivation, the DID cannot be utilized again and trying to resolve it MUST throw a 'DidDeactivated' error.

---

A tyronZIL DID-Deactivate operation consumes approximately 1,500 units of GAS (around 1.5 ZIL, currently less than 0.03 USD).
