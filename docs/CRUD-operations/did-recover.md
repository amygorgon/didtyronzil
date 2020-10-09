# Tyron DID-Recover operation

A Tyron DID-Recover operation fully resets the user's DID-State, so they can keep using the same DID with a brand new public-key-infrastructure (PKI). To be able to execute this operation, the user MUST possess the private [***did_recovery_key***](../protocol-parameters.md#did-keys).

Once the operation request is processed, the client makes a call to the ***DidRecover*** transition of the user's [DID-SC](../smart-contracts/DID-SC.md).

## On the client's side:

0. Initialize with the Zilliqa network (mainnet or testnet) & the user's DID-SC address to fetch the DID-State from the blockchain.
1. Get the user's private ***did_recovery_key*** and verify that corresponds to the public ***did_recovery_key*** stored in the DID-SC.
2. Get the user's input regarding new cryptographic keys and service endpoints for the recovered DID.
3. Verification methods: With the public key input generate an array of keys of type [PublicKeyModel](../implementation/models.md#public-key-model), generated using the [operation key pair](../protocol-parameters.md#operation-key-pair).
3. Service endpoints: An array of endpoints of type [DidServiceEndpointModel](../implementation/models.md#did-service-endpoint-model).
4. With the verification methods and service endpoints, generate a new document of type [DocumentModel](../implementation/models.md#document-model) and hex-encode it.
5. Sign the new document with the pair ***did_recovery_keys***. 
6. Generate new [DID-Keys](../protocol-parameters.md#did-keys) using the [KEY_ALGORITH](../protocol-parameters.md#operation-key-algorithm): The update key-pair (necessary for the following Update operation) & the recovery key-pair (needed for any future Recovery or Deactivate operation).
7. Return the new document, Schnorr signature & public DID-Keys as hex-encoded strings.

    > All private keys MUST be in control of the user.

10. Produce the ***DidRecover*** transition parameters with the output from step 7.
11. The client MUST submit the tyronZIL transaction by calling the ***DidRecover*** transition of the user's DID-SC. For that, the client MUST provide their private key and choose the gas limit.

## On the DID-Smart-Contract's side

When the ***DidRecover*** transition gets called with the proper hex-encoded arguments (new document of type ByStr, signature of type ByStr64 & new [DID-Keys](../protocol-parameters.md#did-keys) of type ByStr33), the DID-SC proceeds as follows:

1. First, it executes the  ***Payment*** procedure to make the DID-SC work only if the payment is correct.
2. Executes the ***IsClient*** procedure to verify that the call comes from the user's client.
3. Executes the ***IsRightStatus*** procedure to verify that the DID-Status is neither ***Deactivated*** nor ***Initialized***.
4. Performs the ***IsRightSignature*** procedure to verify that the signature that comes with the signed document got produced with the ***did_recovery_keys*** by applying the Schnorr signature algorithm. For this verification, the DID-SC utilizes the public key that it has stored in the ***did_recovery_key*** mutable field. 
5. Saves the new document in the ***did_document*** field.
6. Applies the ***IsValidKey*** procedure to verify that all new DID-Keys are unique (and different from before), and then sets the fields ***did_update_key*** & ***did_recovery_key*** with the new values.
7. Sets the DID-Status to ***Recovered***.
8. Executes the ***Timestamp*** procedure.

---

A tyronZIL DID-Recover transaction consumes approximately 700 units of GAS (1.4 ZIL, currently less than 0.03 USD).
