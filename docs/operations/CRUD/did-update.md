# tyronZIL DID-Update operation

A tyronZIL DID-Update operation is conformant with a Sidetree Update Operation, and its goal is to update a tyronZIL DID according to a predetermined set of patches.

Follow these steps:

## 0. User input

Ask the user for the update-private key corresponding to the previous update-commitment, as well as the address of the user's Tyron-Smart-Contract(TSM) to update their DID and the corresponding DID-State. The client MUST fetch the TSM-State and check that the key given by the user corresponds to the update-commitment in the TSM-State.

## 1. Patch action

The user can choose among these [patch actions](../../implementation/models.md#patch-action).

## 2. New update-commitment

2.1 Generate the update keys:

```[UPDATE_KEY, UPDATE_PRIVATE_KEY]```

2.2 Generate the new update-commitment:

```UPDATE_COMMITMENT``` = [Multihash](../../sidetree.md#hash-protocol).canonicalize[ThenHash](../../sidetree.md#commitment-hash)[ThenEncode](../../sidetree.md#data-encoding-scheme)(```UPDATE_KEY```)

## 3. Generate the [DID-state patch](../../sidetree.md#did-state-patch) array

```js
PATCH = {  
    action: PatchAction,
    public_keys?: PublicKeyModel[] | string[];   //array of key to add OR key-ids to remove
    ids?: string [];   //array of service ids to remove those services
    service_endpoints?: ServiceEndpointModel[];         //when adding services
}[]
```

> ```PATCH``` is of type [PatchModel](../../implementation/models.md#patch-model), and the action is any of the [PatchAction](../../implementation/models.md#patch-action) variants from step 1

## 4. Update Operation Delta Object

4.1 Using the DID-State patch and the update-commitment, generate an instance of the Update Operation Delta Object as follows:

```js
DELTA = {
    patches: PATCH,  
    updateCommitment: UPDATE_COMMITMENT  
}
```

> ```DELTA``` is of type [DeltaModel](../../implementation/models.md#delta-model)

Then apply the following operations to the object:

4.2 Stringify it and turn it into a buffer  
4.3 Encode it with the [data encoding scheme](../../sidetree.md#data-encoding-scheme) as ```ENCODED_DELTA```  
4.4 Hash it with the [hash algorithm](../../sidetree.md#hash-algorithm) & [hash protocol](../../sidetree.md#hash-protocol), and then encode it again as ```DELTA_HASH```

## 5. Update Operation Signed Data Object

5.1 Using the ```DELTA_HASH```, and the ```UPDATE_KEY``` from the previous update operation, generate an instance of the Update Operation Signed Data Object as follows:

```js
SIGNED_DATA = {  
    delta_hash: DELTA_HASH,
    update_key: PREVIOUS_UPDATE_KEY 
}
```

> ```SIGNED_DATA``` is of type [UpdateSignedDataModel](../../implementation/models.md#update)

Then sign this object as a e256k compact JWS, signing it with the private-key corresponding to the previous update-commitment.

```ts
const SIGNED_DATA_JWS = await Cryptography.signUsingEs256k(SIGNED_DATA, previousUpdatePrivateKey);
```

## 6. Sidetree request

6.1 Generate the following object:

```js
SIDETREE_REQUEST = {  
    did_suffix: did_tyronZIL,
    signed_data: SIGNED_DATA_JWS,
    type: OperationType.Update,
    delta: ENCODED_DELTA
}
```

6.2 Stringify it and turn it into a buffer as ```SIDETREE_REQUEST_BUFFER```  
6.3 Process the ```SIDETREE_REQUEST_BUFFER``` with the Sidetree's library UpdateOperation, which returns a class with the following properties:

- The original request buffer sent by the requester:

```operationBuffer: Buffer```

- The unique suffix of the DID, globally unique:

```didUniqueSuffix: string```

- The data used to generate the Update Operation Signed Data Object:

```signedDataModel: UpdateSignedDataModel```

- The compact JWS signature that updated the DID

```signedDataJws: Jws```

- The Update Operation Delta Object:

```delta: DeltaModel```

- The encoded delta:

```encodedDelta: string```

## 7. Tyron-Smart-Contract(TSM)

Once the Sidetree Update Operation has been successful, continue by retrieving the updated DID-Document. Then the client MUST submit a transaction that calls the 'DidUpdate' transition of the user's TSM. The required parameters are the previous update-commitment, the new Base64URL-encoded DID-document and the new update-commitment.

---

A tyronZIL DID-Update operation consumes approximately 2,500 units of GAS (around 2.5 ZIL, currently less than 0.05 USD).
