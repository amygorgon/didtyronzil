# tyronZIL DID-Recover operation

A tyronZIL DID-Recover operation is conformant with a Sidetree Recover Operation, and its goal is to recover a tyronZIL DID and fully reset its DID-state, except for the Decentralized Identifier itself. This way, the user can keep using the same DID with brand new PKI.

Follow these steps:

## 0. User input

Ask the user for the recovery-private key corresponding to the previous recovery-commitment, as well as the address of the user's Tyron-Smart-Contract(TSM) to recover their DID. The client MUST fetch the TSM-State and check that the key given by the user corresponds to the recovery-commitment in the TSM-State.

## 1. Verification methods

Generate new 'publicKey' verification methods.

1.1 Under the property 'publicKey', assing to its value an array of keys of type [PublicKeyModel](../../implementation/models.md#public-key-model), generated using the [operation key pair](../../sidetree.md#operation-key-pair):

```publicKeys: PublicKeyModel[]```

## 2. Public-key commitments

2.1 For the [public-key commitments](../../sidetree.md#public-key-commitment), first, generate two key-pairs:

```[UPDATE_KEY, UPDATE_PRIVATE_KEY]```

```[RECOVERY_KEY, RECOVERY_PRIVATE_KEY]```

2.2 Use the ```UPDATE_KEY``` to generate the update-commitment:

```UPDATE_COMMITMENT``` = [Multihash](../../sidetree.md#hash-protocol).canonicalize[ThenHash](../../sidetree.md#commitment-hash)[ThenEncode](../../sidetree.md#data-encoding-scheme)(```UPDATE_KEY```)

2.2 Use the ```RECOVERY_KEY``` to generate the recovery-commitment:

```RECOVERY_COMMITMENT``` = [Multihash](../../sidetree.md#hash-protocol).canonicalize[ThenHash](../../sidetree.md#commitment-hash)[ThenEncode](../../sidetree.md#data-encoding-scheme)(```RECOVERY_KEY```)

## 3. Service endpoints

Under the property 'service', assign to its value an array of service endpoint objects of type [ServiceEndpointModel](../../implementation/models.md#service-endpoint-model):

```service: ServiceEndpointModel[]```

## 4. Document model

Generate a document of type [DocumentModel](../../implementation/models.md#document-model):

```js
DOCUMENT = {  
    public_keys: publicKeys,  
    service_endpoints: service  
}
```

## 5. DID-State patch

Put the previously generated document inside of a [DID-State patch](../../sidetree.md#did-state-patch). For the DID-Recover operation, the patch action is 'Replace':

```js
PATCH = {  
    action: PatchAction.Replace,  
    document: DOCUMENT  
}
```

> ```PATCH``` is of type [PatchModel](../../implementation/models.md#patch-model), with a Replace [PatchAction](../../implementation/models.md#patch-action)

## 6. Recovery Operation Delta Object

6.1 Using the DID-State patch and the update-commitment, generate an instance of the Recovery Operation Delta Object as follows:

```js
DELTA = {
    patches: [PATCH],  
    updateCommitment: UPDATE_COMMITMENT  
}
```

> ```DELTA``` is of type [DeltaModel](../../implementation/models.md#delta-model)

Then apply the following operations to the object:

6.2 Stringify it and turn it into a buffer  
6.3 Encode it with the [data encoding scheme](../../sidetree.md#data-encoding-scheme) as ```ENCODED_DELTA```  
6.4 Hash it with the [hash algorithm](../../sidetree.md#hash-algorithm) & [hash protocol](../../sidetree.md#hash-protocol), and then encode it again as ```DELTA_HASH```

## 7. Recovery Operation Signed Data Object

7.1 Using the ```DELTA_HASH```, the ```RECOVERY_KEY``` corresponding to the previous recovery-commitment and the new recovery-commitment, generate an instance of the Recovery Operation Signed Data Object as follows:

```js
SIGNED_DATA = {  
    delta_hash: DELTA_HASH,
    recovery_key: PREVIOUS_RECOVERY_KEY,
    recovery_commitment: NEW_RECOVERY_COMMITMENT  
}
```

> ```SIGNED_DATA``` is of type [RecoverSignedDataModel](../../implementation/models.md#recover)

7.2 Then sign this object as a e256k compact JWS, signing it with the private-key corresponding to the previous recovery-commitment.

```ts
const SIGNED_DATA_JWS = await Cryptography.signUsingEs256k(SIGNED_DATA, previousRecoverPrivateKey);
```

## 8. Sidetree request

8.1 Generate the following object:

```js
SIDETREE_REQUEST = {  
    did_suffix: did_tyronZIL,
    signed_data: SIGNED_DATA_JWS,
    type: OperationType.Recover,
    delta: ENCODED_DELTA
}
```

8.2 Stringify it and turn it into a buffer as ```SIDETREE_REQUEST_BUFFER```  
8.3 Process the ```SIDETREE_REQUEST_BUFFER``` with the Sidetree's library RecoverOperation, which returns a class with the following properties:

- The original request buffer sent by the requester:

```operationBuffer: Buffer```

- The unique suffix of the DID, globally unique:

```didUniqueSuffix: string```

- The data used to generate the Recovery Operation Signed Data Object:

```signedDataModel: RecoverSignedDataModel```

- The compact JWS signature that recovered the DID

```signedDataJws: Jws```

- The Recovery Operation Delta Object:

```delta: DeltaModel```

- The encoded delta:

```encodedDelta: string```

## 9. Tyron-Smart-Contract(TSM)

Once the Sidetree Recover Operation has been successful, continue by retrieving the DID-Document from the encoded delta. Then the client MUST submit a transaction that calls the 'DidRecover' transition of the user's TSM. The required parameters are the previous recovery-commitment, the new Base64URL-encoded DID-document, the new update-commitment and the new recovery-commitment.

---

A tyronZIL DID-Recover operation consumes approximately 3,500 units of GAS (around 3.5 ZIL, currently less than 0.07 USD).
