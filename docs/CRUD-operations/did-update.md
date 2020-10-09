# Tyron DID-Update operation

A Tyron DID-Update operation makes modifications to the user's DID-Document that exists in their DID-SC. To be able to execute this operation, the user MUST possess the private [***did_update_key***](../protocol-parameters.md#did-keys).


Once the operation request is processed, the client makes a call to the ***DidUpdate*** transition of the user's [DID-SC](../smart-contracts/DID-SC.md).


## On the client's side:

0. Initialize with the Zilliqa network (mainnet or testnet) & the user's DID-SC address to fetch the DID-State from the blockchain.
1. Get the user's private ***did_update_key*** and verify that is matching the public ***did_update_key*** stored in the DID-SC.
2. Get the user's input regarding the update [patch actions](../implementation/models.md#patch-action) and generate a patches array.
3. Process the patches array along with the former document retrieved from the DID-SC, to obtain an updated-document and hex-encode it.
4. Sign the new document with the pair ***did_update_keys***. 
6. Generate a new key-pair for the ***did_update_keys*** necessary for any future Update operation.
7. Return the new document, Schnorr signature & the new public ***did_update_key*** as hex-encoded strings.

    > All private keys MUST be in control of the user.

10. Produce the ***DidUpdate*** transition parameters with the output from step 7.
11. The client MUST submit the tyronZIL transaction by calling the ***DidUpdate*** transition of the user's DID-SC. For that, the client MUST provide their private key and choose the gas limit.

## On the DID-Smart-Contract's side

When the ***DidUpdate*** transition gets called with the proper hex-encoded arguments (new document of type ByStr, signature of type ByStr64 & new public ***did_update_key*** of type ByStr33), the DID-SC proceeds as follows:

1. First, it executes the  ***Payment*** procedure to make the DID-SC work only if the payment is correct.
2. Executes the ***IsClient*** procedure to verify that the call comes from the user's client.
3. Executes the ***IsRightStatus*** procedure to verify that the DID-Status is neither ***Deactivated*** nor ***Initialized***.
4. Performs the ***IsRightSignature*** procedure to verify that the signature that comes with the signed document got done with the ***did_update_keys*** by applying the Schnorr signature algorithm. The DID-SC utilizes for this verification the public key that it has stored in the ***did_update_key*** mutable field. If the procedure is successful, then the transition saves the updated-document in the ***did_document*** field.
5. Applies the ***IsValidKey*** procedure to verify that the new public ***did_update_key*** is different from any other key. If so, then saves the new key in the ***did_update_key*** mutable field.
6. Sets the DID-Status to ***Updated***.
9. Executes the ***Timestamp*** procedure.

---

A tyronZIL DID-Update transaction consumes approximately 700 units of GAS (1.4 ZIL, currently less than 0.03 USD).
