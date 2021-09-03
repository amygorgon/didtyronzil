# DID Update transaction

It makes modifications to the user's self-sovereign identity (stored in their DID smart contract). To be able to execute this operation, the user MUST possess the private [*did_update_key*](../protocol-parameters.md#did-keys).

Once the operation request is processed, the contract owner MUST make a call to the *DidUpdate* transition of the user's DID smart contract.

## On the SSI Client's side:

0. Initialize with the Zilliqa network (mainnet or testnet) & the user's domain name.did to fetch their DID contract from the blockchain.
1. Utilize the user's private *did_update_key* to verify that it corresponds to the public *did_update_key* stored in the DID smart contract.
2. Get the user's input regarding the update [patch actions](../implementation/models.md#patch-action), and generate the *List Document*.
3. Hash the document with the [HASH_ALGORITHM](../protocol-parameters.md#hash-algorithm) and sign the hash with the [SIGNATURE_ALGORITHM](../protocol-parameters.md#signature-algorithm) using the *did_update_keys*. 
4. Generate a new key-pair for the *did_update_keys* necessary for any future Update operation.
5. Return the new document, hash, Schnorr signature & the new public *did_update_key*.

    > All private keys MUST be in control of the user.

6. Submit the *DidUpdate* tyronzil transaction with its transition parameters (agent, newDocument, docHash, signature and newUpdateKey). The contract owner MUST call this transition for it to be successful.

## On the DID smart contract's side

When the *DidUpdate* transition gets called, the DID smart contract proceeds as follows:

0. First, it verifies that the DID Status is *operational* by executing the *IsRightStatus* procedure.
1. Executes the *Payment* procedure to make the Self-Sovereign Identity work only if the payment is correct.
2. Executes the *IsRightCaller* procedure to verify that the call comes from the user (contract owner address).
4. Performs the *IsRightSignature* procedure to verify that the signature got produced with the *did_update_keys* by applying the Schnorr signature algorithm. For this verification, the DID contract utilizes the public key that it has stored in the *did_update_key* mutable field.
5. Executes the *UpdateDocument* procedure to update the *verification_methods* and *services* map fields.
6. Applies the *IsValidKey* procedure to verify that the new public *did_update_key* is different from any other key. If so, then saves the new key in the *did_update_key* mutable field.
7. Sets the DID Status to *Updated*.
8. Emits a "DID_updated" event & executes the *Timestamp* procedure.

---

A DID Update tyronzil transaction consumes approximately 700 units of GAS (1.4 ZIL).
