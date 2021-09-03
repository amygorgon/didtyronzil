# DID Deactivate transaction

It completely deactivates the Decentralized Identifier by setting the DID Status to *Deactivated* and the DID state to *None*, *undefined* or *Empty*.

To be able to execute this operation, the user MUST possess the private [*did_recovery_key*](../protocol-parameters.md#did-keys).

Once the operation request is processed, the user as the contract owner MUST call the *DidDeactivate* transition of their DID smart contract. This transition is irreversible - after deactivation, the DID contract will never be useful again. Resolving the Decentralized Identifier in future occasions MUST throw a *DidDeactivated* error.

## On the SSI Client's side:

0. Initialize with the Zilliqa network (mainnet or testnet) & the user's domain name.did to fetch their DID contract from the blockchain.
1. Utilize the user's private *did_recovery_key* to verify that it corresponds to the public *did_recovery_key* stored in the DID smart contract.
3. Retrieve the [*tyron_hash*](../protocol-parameters.md#tyron-hash) from the user's DID contract and sign it with the pair *did_recovery_keys* through the [SIGNATURE_ALGORITHM](../protocol-parameters.md#signature-algorithm).
5. Submit the *DidDeactivate* tyronzil transaction with its transition parameters (agent and Schnorr signature). The contract owner MUST call this transition for it to be successful.

## On the DID smart contract's side

When the *DidDeactivate* transition gets called with the proper hex-encoded signature of type ByStr64 as its argument, the DID smart contract proceeds as follows:

0. First, it verifies that the DID Status is *operational* by executing the *IsRightStatus* procedure.
1. Executes the  *Payment* procedure to make the Self-Sovereign Identity work only if the payment is correct.
2. Executes the *IsRightCaller* procedure to verify that the call comes from the user (contract owner address).
3. Fetches the *did_recovery_key* and the *tyron_hash* from the contract's state.
4. Performs the *IsRightSignature* procedure. It verifies that the signature got produced with the *did_recovery_keys* by applying the Schnorr signature algorithm on the *tyron_hash*. 5. Sets the DID Status to *Deactivated*, the *verification_methods* to *Emp String ByStr33*, the *services* to *Emp String Pair String String* and both the *did_update_key* & *did_recovery_key* to None{ByStr33}.
6. Emits the "DID_deactivated" event & executes the *Timestamp* procedure.

---

A DID Deactivate tyronzil transaction consumes approximately 500 units of GAS (1 ZIL).
