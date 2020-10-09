# Tyron DID-Create operation

A Tyron DID-Create operation generates a brand new Decentralized Identifier, its [DID-Document](../did-document.md) and DID-State, and stores these data in a new [DID-Smart-Contract (DID-SC)](../smart-contracts/DID-SC.md).

## On the client's side:

0. Initialize with the Zilliqa network (mainnet or testnet), the user's address, the client's private key and the gas limit.
1. Get the user's input regarding cryptographic keys and service endpoints for the DID.
2. Verification methods: With the public key input generate an array of keys of type [PublicKeyModel](../implementation/models.md#public-key-model), generated using the [operation key pair](../protocol-parameters.md#operation-key-pair).
3. Service endpoints: An array of endpoints of type [DidServiceEndpointModel](../implementation/models.md#did-service-endpoint-model).
4. With the verification methods and service endpoints, generate a document of type [DocumentModel](../implementation/models.md#document-model) and hex-encode it.
5. Sign the document with the ***did_contract_owner*** key-pair. 
6. Generate the [DID-Keys](../protocol-parameters.md#did-keys) using the [KEY_ALGORITH](../protocol-parameters.md#operation-key-algorithm): The update key-pair (necessary for the following Update operation) & the recovery key-pair (needed for any future Recovery or Deactivate operation).
7. Return the document, Schnorr signature & public DID-Keys as hex-encoded strings.

    > All private keys MUST be in control of the user.

8. Download, decode (Base64) & decompress the DID-SC-template from the ***init.tyron*** smart-contract.
9. Instantiate the DID-SC with the user's address as the ***contract_owner*** and deploy it on the Zilliqa blockchain platform.
10. Call the [***ContractInit***](../smart-contracts/DID-SC.md#transitions) transition of the user's DID-SC.
11. Produce the ***DidCreate*** transition parameters with the output from step 7.
12. The client MUST submit the tyronZIL transaction by calling the ***DidCreate*** transition of the user's DID-SC.

## On the DID-Smart-Contract's side

When the ***DidCreate*** transition gets called with the proper hex-encoded arguments (document of type ByStr, signature of type ByStr64 & [DID-Keys](../protocol-parameters.md#did-keys) of type ByStr33), the DID-SC proceeds as follows:

1. First, it executes the  ***Payment*** procedure to make the DID-SC work only if the payment is correct.
2. Executes the ***IsClient*** procedure to verify that the call comes from the user's client.
3. Executes the ***IsInitialized*** procedure to verify that the DID-Status is ***Initialized***.
4. Executes the ***DidScheme*** procedure, which generates the user's Decentralized Identifier according to the [DID-Scheme](../scheme/did-scheme.md) with the DID-SC address as the [DID-Suffix](../protocol-parameters.md#did-suffix). This procedure also produces the ***tyron_hash*** by applying the [HASH_ALGORITH](../protocol-parameters.md#hash-algorithm) to the DID.
5. Executes the ***IsOwnerKey*** procedure to verify that the ***did_contract_owner*** public key matches the DID-SC's ***contract_owner*** address.
6. Performs the ***IsRightSignature*** procedure to verify that the signature that comes with the signed document got done with the ***did_contract_owner*** key-pair by applying the Schnorr signature algorithm. If it's correct, then saves the hex-encoded-document in the ***did_document*** field.
7. Applies the ***IsValidKey*** procedure to verify that all DID-Keys are unique, and then sets the fields ***did_update_key*** & ***did_recovery_key***.
8. Sets the DID-Status to *Created* and saves the blocknumber into the ***created*** field.
9. Executes the ***Timestamp*** procedure.

---

A tyronZIL DID-Create transaction (incl. the DID-SC deployment and initialization) consumes approximately 2,100 units of GAS (4.2 ZIL, currently less than 0.09 USD).
