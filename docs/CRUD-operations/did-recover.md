# DID Recover transaction

It fully resets the user's DID State, so they can keep using the same DID with a brand new public-key-infrastructure (PKI). To be able to execute this operation, the user MUST possess the private [*did_recovery_key*](../protocol-parameters.md#did-keys).

Once the operation request is processed, the user as the contract owner MUST call the *DidRecover* transition of the user's DID smart contract.

## On the SSI Client's side:

0. Initialize with the Zilliqa network (mainnet or testnet) & the user's domain name.did to fetch the DID contract from the blockchain.
1. Utilize the user's private *did_recovery_key* to verify that it corresponds to the public *did_recovery_key* stored in the DID contract. 
2. Verification methods: With the public key input generate an array of keys, generated using the [operation key pair](../protocol-parameters.md#operation-key-pair).
3. Service endpoints: An array of endpoints for the recovered DID.
4. With the verification methods and service endpoints, generate a new *List Document* to send to the DID smart contract.
5. Hash the document with the [HASH_ALGORITHM](../protocol-parameters.md#hash-algorithm) and sign the hash with the [SIGNATURE_ALGORITHM](../protocol-parameters.md#signature-algorithm) using the *did_recovery_keys*. 
6. Generate new [DID Keys](../protocol-parameters.md#did-keys) using the [KEY_ALGORITH](../protocol-parameters.md#operation-key-algorithm): The update key-pair (necessary for the following *Update* operation) & the recovery key-pair (needed for any future *Recover* or *Deactivate* operation).
7. Return the new document, Schnorr signature & public DID Keys.

    > All private keys MUST be in control of the user.

8. Submit the *DidRecover* tyronzil transaction with its transition parameters (agent, newDocument, docHash, signature, newUpdateKey and newRecoveryKey). The contract owner MUST call this transition for it to be successful.

## On the DID Smart Contract's side

When the *DidRecover* transition gets called, the DID smart contract proceeds as follows:

0. First, it verifies that the DID Status is *operational* by executing the *IsRightStatus* procedure.
1. Executes the  *Payment* procedure to make the Self-Sovereign Identity work only if the payment is correct.
2. Executes the *IsRightCaller* procedure to verify that the call comes from the user (contract owner address).
3. Performs the *IsRightSignature* procedure to verify that the signature got produced with the *did_recovery_keys* by applying the Schnorr signature algorithm. For this verification, the DID contract utilizes the public key that it has stored in the *did_recovery_key* mutable field.
4. Executes the *UpdateDocument* procedure to re-initialize the *verification_methods* and *services* map fields.
6. Applies the *IsValidKey* procedure to verify that all new DID Keys are unique (and different from before), and then sets the fields *did_update_key* & *did_recovery_key* with the new values.
7. Sets the DID Status to *Recovered*.
8. Emits a "DID_recovered" event & executes the *Timestamp* procedure.

---

A DID Recover tyronzil transaction consumes approximately 700 units of GAS (1.4 ZIL).
