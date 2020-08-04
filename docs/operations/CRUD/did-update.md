# tyronZIL DID-update operation

A tyronZIL DID-create operation is conformant with a Sidetree Update operation, and its goal is to update a tyronZIL DID according to a predetermined set of patches.

Follow these steps:

## 0. User input

Get the DID to update and the corresponding update private key.

## 1. Patch action

The user can choose among these [patch actions](../../implementation/models.md#patch-action).

## 2. New update commitment

2.1 Generate the update keys:

```[UPDATE_KEY, UPDATE_PRIVATE_KEY]```

2.2 Generate the new update commitment:

```UPDATE_COMMITMENT``` = [Multihash](../../sidetree.md#hash-protocol).canonicalize[ThenHash](../../sidetree.md#commitment-hash)[ThenEncode](../../sidetree.md#data-encoding-scheme)(```UPDATE_KEY```)

2.3 Generate the corresponding Operation verification method

## 3. Generate the [DID-state patch](../../sidetree.md#did-state-patch)

```js
PATCH = {  
    action: PatchAction,
    public_keys?: PublicKeyModel[] | string[];  // array of key ids to remove those keys
    ids?: string [];    // array of service ids to remove those services
    service_endpoints?: ServiceEndpointModel[];         // when adding services
}
```

> ```PATCH``` is of type [PatchModel](../../implementation/models.md#patch-model), and the action any of the [PatchAction](../../implementation/models.md#patch-action) variants from step 1

## 4. Update Operation Delta Object

4.1 Using the DID state patch and the update-commitment, generate an instance of the Update Operation Delta Object as follows:

```js
DELTA = {
    patches: [PATCH],  
    updateCommitment: UPDATE_COMMITMENT  
}
```

> ```DELTA``` is of type [DeltaModel](../../implementation/models.md#delta-model)

Then apply the following operations to the object:

4.2 Stringify it and turn it into a buffer  
4.3 Encode it with the [data encoding scheme](../../sidetree.md#data-encoding-scheme) as ```ENCODED_DELTA```  
4.4 Hash it with the [hash algorithm](../../sidetree.md#hash-algorithm) & [hash protocol](../../sidetree.md#hash-protocol) and then encode it again as ```DELTA_HASH```

## 5. Update Operation Signed Data Object

5.1 Using this ```DELTA_HASH``` and the ```UPDATE_KEY``` from the previous update operation, generate an instance of the Update Operation Signed Data Object as follows:

```js
SIGNED_DATA = {  
    delta_hash: DELTA_HASH,
    update_key: PREVIOUS_UPDATE_KEY,
    recovery_commitment: RECOVERY_COMMITMENT  
}
```

> ```SIGNED_DATA``` is of type [UpdateSignedDataModel](../../implementation/models.md#update)

Then sign this object as a e256k compact JWS, signing it with the private key from the previous update operation (user input).

```ts
const SIGNED_DATA_JWS = await Cryptography.signUsingEs256k(SIGNED_DATA, input.updatePrivateKey);
```

## 6. Sidetree request

6.1 Generate the following object:

```js
SIDETREE_REQUEST = {  
    did_suffix: did_tyronZIL.didUniqueSuffix,
    signed_data: SIGNED_DATA_JWS,
    type: OperationType.Update,
    delta: ENCODED_DELTA
}
```

6.2 Stringify it and turn it into a buffer as ```OPERATION_BUFFER```  
6.3 Send the ```OPERATION_BUFFER``` to the Sidetree's library UpdateOperation, which returns a class with the following properties:

- The original request buffer sent by the requester:

```operationBuffer: Buffer```

- The unique suffix of the DID, globally unique:

```didUniqueSuffix: string```

- The data used to generate the Update Operation Signed Data Object:

```signedDataModel: UpdateSignedDataModel```

- The compact JWS signature that updated the DID

```signedDataJws: Jws```

- The Update Operation Delta Object:

```delta: DeltaModel | undefined```

- The encoded string of the delta:

```encodedDelta: string | undefined```

## 7. tyronZIL DID-update operation result

The return value of a tyronZIL-js DID-update operation is an instance of the class [DidUpdate](https://github.com/julio-cabdu/tyronZIL-js/tree/master/src/lib/did-operations/did-update.ts), which includes all the previously mentioned properties plus additional such as the DID in format tyronZIL DID-scheme, the Operation verification method, the updated DID-state, private keys, the update key and commitment.

```type: OperationType.Update```
