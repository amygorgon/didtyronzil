# tyronZIL DID-recover operation

A tyronZIL DID-recover operation is conformant with a Sidetree Recover operation, and its goal is to recover a tyronZIL DID and reset its DID-state. This way the user can keep using the same Decentralized Identifier with brand new PKI.

Follow these steps:

## 0. User input

Get the DID to recover and the corresponding recovery private key.

## 1. Verification methods

Generate the 3 tyronZIL-supported verification methods: 'publicKey', 'operation' and 'recovery'.

1.1 Under the property 'publicKey', assing to its value an array of keys of type [PublicKeyModel](../../implementation/models.md#public-key-model), generated using the [operation key pair](../../sidetree.md#operation-key-pair):

```publicKeys: PublicKeyModel[]```

1.2 Under the property 'operation', assign to its value an [Operation](../../implementation/models.md#sidetree-verification-methods) verification method object:

```operation: Operation```

1.3 Under the property 'recovery', assign to its value a [Recovery](../../implementation/models.md#sidetree-verification-methods) verification method object:

```recovery: Recovery```

## 2. Public key commitments

2.1 For the [public key commitments](../../sidetree.md#public-key-commitment), first, generate two key-pairs:

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

## 5. DID-state patch

Put the previously generated document inside of a [DID state patch](../../sidetree.md#did-state-patch). For the DID-recover operation, the patch action is 'replace':

```js
PATCH = {  
    action: PatchAction.Replace,  
    document: DOCUMENT  
}
```

> ```PATCH``` is of type [PatchModel](../../implementation/models.md#patch-model), with a Replace [PatchAction](../../implementation/models.md#patch-action)

## 6. The Recovery Operation Delta Object

6.1 Using the DID-state patch and the update-commitment, generate an instance of the Recovery Operation Delta Object as follows:

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
6.4 Hash it with the [hash algorithm](../../sidetree.md#hash-algorithm) & [hash protocol](../../sidetree.md#hash-protocol) and then encode it again as ```DELTA_HASH```

## 7. The Recovery Operation Signed Data Object

7.1 Using this ```DELTA_HASH```, the ```RECOVERY_KEY``` from the previous recover operation and the new recovery-commitment, generate an instance of the Recover Operation Signed Data Object as follows:

```js
SIGNED_DATA = {  
    delta_hash: DELTA_HASH,
    recovery_key: PREVIOUS_RECOVERY_KEY,
    recovery_commitment: RECOVERY_COMMITMENT  
}
```

> ```SIGNED_DATA``` is of type [RecoverSignedDataModel](../../implementation/models.md#recover)

7.2 Then sign this object as a e256k compact JWS, signing it with the private key from the previous recover operation (user input).

```ts
const SIGNED_DATA_JWS = await Cryptography.signUsingEs256k(SIGNED_DATA, recoverPrivateKey);
```

## 8. Sidetree request

8.1 Generate the following object:

```js
SIDETREE_REQUEST = {  
    did_suffix: did_tyronZIL.didUniqueSuffix,
    signed_data: SIGNED_DATA_JWS,
    type: OperationType.Recover,
    delta: ENCODED_DELTA
}
```

8.2 Stringify it and turn it into a buffer as ```OPERATION_BUFFER```  
8.3 Send the ```OPERATION_BUFFER``` to the Sidetree's library RecoverOperation, which returns a class with the following properties:

- The original request buffer sent by the requester:

```operationBuffer: Buffer```

- The unique suffix of the DID, globally unique:

```didUniqueSuffix: string```

- The data used to generate the Recover Operation Signed Data Object:

```signedDataModel: RecoverSignedDataModel```

- The compact JWS signature that recovered the DID

```signedDataJws: Jws```

- The Recover Operation Delta Object:

```delta: DeltaModel | undefined```

- The encoded string of the delta:

```encodedDelta: string | undefined```

## 9. tyronZIL DID-recover operation result

The return value of a tyronZIL-js DID-recover operation is an instance of the class [DidRecover](https://github.com/julio-cabdu/tyronZIL-js/tree/master/src/lib/did-operations/did-recover.ts), which includes all the previously mentioned properties plus additional such as the DID in format tyronZIL DID-scheme, the Operation and Recovery verification method, commitments, public and private keys.

```type: OperationType.Recover```
