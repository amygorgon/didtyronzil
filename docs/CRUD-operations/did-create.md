# DID Create transaction

It generates a brand new Self-Sovereign Identity out of the DID smart contract code. It defines the Decentralized Identifier, its [DID Document](../did-document.md) and DID State.

## On the SSI client's side:

0. Initialize with the Zilliqa network (mainnet or testnet), the user's private key and the gas limit.
1. Get the user's input regarding cryptographic keys and service endpoints for the DID Document.
2. Verification methods: With the public key input generate an array of keys, generated using the [operation key pair](../protocol-parameters.md#operation-key-pair).
3. Services: An array of endpoints.
4. With the verification methods and service endpoints, generate a *List Document* to send to the DID smart contract.
5. Generate the [DID Keys](../protocol-parameters.md#did-keys) using the [KEY_ALGORITH](../protocol-parameters.md#operation-key-algorithm): The update key-pair (necessary for the following *Update* operation) & the recovery key-pair (needed for any future *Recover* or *Deactivate* operation).

    > All private keys MUST be in control of the user.

6. Download, decode (Base64) & decompress the DID-smart-contract code from the INIT.tyron smart contract.
7. Instantiate the DID contract with the user's Zilliqa address as the contract owner and deploy it on the blockchain.
8. Call the init transition of the user's DID contract.
9. Submit the *DidCreate* tyronzil transaction with its transition parameters (agent, document, updateKey and recoveryKey). The contract owner MUST call this transition for it to be successful.
10. Ask which domain name.did the user would like for their DID contract address and execute the SetSsiDomain transition.

## On the DID smart contract's side

When the *DidCreate* transition gets called, the DID smart contract proceeds as follows:

0. First, it verifies that the DID Status is initialized by executing the *IsRightStatus* procedure.
1. Executes the *Payment* procedure to make the Self-Sovereign Identity work only if the payment is correct.
2. Executes the *IsRightCaller* procedure to verify that the call comes from the user (contract owner address).
3. Executes the *DidScheme* procedure, which generates the user's Decentralized Identifier according to the [DID Scheme](../scheme/did-scheme.md) with the DID contract address as the [DID Suffix](../protocol-parameters.md#did-suffix). This procedure also produces the *tyron_hash* by applying the [HASH_ALGORITH](../protocol-parameters.md#hash-algorithm) to the DID.
4. Executes the *UpdateDocument* procedure to initialize the verification_methods and services map fields.
5. Applies the *IsValidKey* procedure to verify that all DID Keys are unique, and then sets the fields *did_update_key* & *did_recovery_key*.
6. Sets the DID Status to *Created* and saves the blocknumber into the *created* field.
7. Executes the *Timestamp* procedure.

---

A DID Create tyronzil transaction (incl. the DID smart contract deployment and initialization) consumes approximately 2,200 units of GAS (4.4 ZIL).
