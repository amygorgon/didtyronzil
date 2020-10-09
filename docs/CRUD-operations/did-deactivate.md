# Tyron DID-Deactivate operation

A Tyron DID-Deactivate operation completely deactivates the Decentralized Identifier by setting the DID-Status to ***Deactivated*** and the DID-State mutable fields to None.

To be able to execute this operation, the user MUST possess the private [***did_recovery_key***](../protocol-parameters.md#did-keys).

Once the operation request is processed, the client makes the call to the ***DidDeactivate*** transition of the user's [DID-SC](../smart-contracts/DID-SC.md). This transition is irreversible - after deactivation, the DID-SC will never be useful again. Resolving the Decentralized Identifier in future occasions MUST throw a ***DidDeactivated*** error.

## On the client's side:

0. Initialize with the Zilliqa network (mainnet or testnet) & the user's DID-SC address to fetch the DID-State from the blockchain.
1. Get the user's private ***did_recovery_key*** and verify that icorresponds to the public ***did_recovery_key*** stored in the DID-SC.
3. Retrieve the [***tyron_hash***](../protocol-parameters.md#tyron-hash) from the user's DID-SC and sign it with the pair ***did_recovery_keys***. 
4. Return the Schnorr signature as a hex-encoded string, and with it produce the ***DidDeactivate*** transition parameter.
5. The client MUST submit the tyronZIL transaction by calling the ***DidDeactivate*** transition of the user's DID-SC. For that, the client MUST provide their private key and choose the gas limit.

## On the DID-Smart-Contract's side

When the ***DidDeactivate*** transition gets called with the proper hex-encoded signature of type ByStr64 as its argument, the DID-SC proceeds as follows:

1. First, it executes the  ***Payment*** procedure to make the DID-SC work only if the payment is correct.
2. Executes the ***IsClient*** procedure to verify that the call comes from the user's client.
3. Executes the ***IsRightStatus*** procedure to verify that the DID-Status is neither ***Deactivated*** nor ***Initialized***.
4. Performs the ***IsRightSignature*** procedure. It verifies that the signature got produced with the ***did_recovery_keys*** by applying the Schnorr signature algorithm on the ***tyron_hash***. For this verification, the DID-SC utilizes the public key that it has stored in the ***did_recovery_key*** mutable field.
5. Sets the DID-Status to ***Deactivated***, the ***did_document*** to None{ByStr} and both the ***did_update_key*** & ***did_recovery_key*** to None{ByStr33}.
6. Executes the ***Timestamp*** procedure.

---

A tyronZIL DID-Deactivate transaction consumes approximately 500 units of GAS (1 ZIL, currently less than 0.02 USD).
