# didc.tyron: The Decentralized Identifier Smart Contract

The user's DID smart contract (DIDC) gets instantiated from a template that is stored on-chain in the [init.tyron](./init.tyron.md) smart contract. For that, the user MUST give their Zilliqa address as the *contract owner*. Once the DIDC gets deployed, its code is immutable (the code can never get modified under the same contract's address). However, the DIDC has mutable fields declared in its code, and there are predetermined ways, called transitions, that can modify those fields in specific manners.

> Find the [DIDC's code](https://github.com/julio-cabdu/tyronZIL-js/blob/master/src/lib/blockchain/smart-contracts/didc.scilla) on GitHub.

## Immutable fields

Immutable fields are set at deployment and can not get modified. The DIDC gets deployed with the following immutable fields:

0. *initContractOwner*: The Zilliqa address of the user who is the owner of the contract | [ByStr20 type]
1. *initTyron*: The Zilliqa address of the [init.tyron](./init.tyron.md) smart contract | [ByStr20 type]

## Mutable fields

Mutable fields can get modified but only if there is a specific code in the contract that allows it. They MUST get initialized with a value at deployment.

0. *contract_owner*: The Zilliqa address of the user that can get updated by the *UpdateOwner* transition | [ByStr20 type]
1. *decentralized_identifier*: The user's DID generated according to the [DID-Scheme](../scheme/did-scheme.md) - [String type].
2. *init_tyron*: The address of the [init.tyron](./init.tyron.md) contract's latest implementation | [ByStr20 type]
3. *resource_records*: The Decentralized Identifier's DNS resource records | @key: domain | @value: avatar | [Map String String type]
4. *did_agents*: The user's SSI agents | @key: avatar.agent | @value: agent's address | [Map String ByStr20 type]
5. *latest_agent*: The Zilliqa address of the current agent | [ByStr20 type]
6. *did_status*: The DID-Status can be *Undefined*, *Initialized*, *Created*, *Updated*, *Recovered* or *Deactivated* | [DidStatus ADT type]
7. *verification_methods*: The Tyron verification methods are the public keys used by the DIDC | @key: key purpose | @value: public key of type "SchnorrSecP256k1VerificationKey2019" | [Map String ByStr33 type]
8. *services*: The DID service endpoints | @key: ID | @value: service type & URI | [Map String Pair String String type]
9. *tyron_hash*: The SHA256 hash of the DID. It is necessary to sign this hash to perform certain operations, such as deactivation of the DID | [ByStr type]
10. *did_update_key* & *did_recovery_key*: The public [DID-Keys](../protocol-parameters.md#did-keys) to enable future operations are stored in the DIDC to execute the *IsRightSignature* procedure - [ByStr type]
11. *created*: The block number when the DID-Create tyronZIL transaction occurred | [BNum type]
12. *ledger_time*: The block number when the last tyronZIL transaction occurred | [BNum type]
13. *transaction_number*: A monotonically increasing number representing the amount of tyronZIL transactions that have taken place | [Uint128 type]

    The following fields get determined by calling the *Init* transition which sends a message to the [init.tyron](./init.tyron.md) contract that, in return, call the *InitCallBack* transition:

14. *operation_cost*: The cost of each tyronZIL transaction type, e.g. ".did" for DID operations | [Map String Uint128 type] 
15. *foundation_address*: The address of the Tyron Pungtas Foundation | [ByStr20 type]
16. *agent_comission*: The agent's comission as a % of the *operation_cost* | @key: avatar.agent | @value: comission | [Map String Uint 128 type]
17. *latest_comission*: It corresponds to the commission of the current agent | [Uint128 type] 

### The xWallet fields

18. *xWallet*: The SSI Token implementations | @key: token address | @value: token name | [Map ByStr20 String type]
21. *xBalances*: The SSI Token balances | @key: token name | @value: balance | [Map String Uint128 type]
22. *xProxies*: The SSI Token proxies | @key: token name | @value: proxy address | [Map String ByStr20 type]
23. *donation*: The Donation Scheme | @key: campaign name | @value: campaign's starting blok number | [Map String BNum]

## Procedures 

Procedures can change the state of the contract (mutable fields), but they are not part of the public API which means that they can only get invoked from within the contract itself.

1. *DidScheme*: It generates the user's *decentralized_identifier* & *tyron_hash*. This procedure is the only way to generate the Decentralized Identifier, and it can only get executed by the *DidCreate* transition.
2. *ThrowError*: The procedure to throw an error.
3. *IsRightCaller*: Validates that the order comes from a SSI Trinity entity. This procedure matches against a *SsiTrinity* ADT: user (to validate that the call comes from the user/ contract owner address), agent (to verify the agent in the Payment procedure) or tyron (to validate that the call comes from the [init.tyron](./init.tyron.md) contract.
4. *IsRightStatus*: Verifies that the DID-Status is correct for the given operation.
5. *Donation*: Checks that the donation campaign code is within the limitation period of 1-week.
6. *Payment*: Executes the payment to the agents and foundation.
7. *IsRightSignature*: Verifies the Schnorr signature that comes with the signed data corresponds with the public DID-Keys stored in the contract.
8. *UpdateDocument*: Updates the verification methods and services map fields.
9. *IsValidKey*: Verifies that the new DID-Key is unique and not utilized more than once.
11. *Timestamp*: Generates a timestamp that sets the *ledger_time* & *transaction_number*.

### The xWallet procedure

12. *XWallet*: Verifies the validity, checks & updates the balance of the SSI Tokens

## Transitions

Transitions are the public API of the DIDC and get invoked by sending messages to the contract.  

0. *UpdateOwner*: Updates the Zilliqa address of the user aka contract owner.
1. *UpdateInit*: Updates the [init.tyron](./init.tyron.md) contract to the address of its latest implementation - only the user can execute this transition.
1. *Init*: Sets the agent's info into the user's DIDC & sends a message to the [init.tyron](./init.tyron.md) contract that, in return, calls the *InitCallBack* transition. It also sets the *did_status* to Initialized.
2. *InitCallBack*: The [init.tyron](./init.tyron.md) contract calls this transition to set the *operation_cost*, *foundation_address* & address and commission of the given agent.
3. *SetSsiDomain*: Sets a domain name for the Decentralized Identifier.
4. *SetDomainCallBack*: The [init.tyron](./init.tyron.md) contract calls this transition to set a user's domain name.
5. *DidCreate*: Executes a [DID-Create](../CRUD-operations/did-create.md) tyronZIL transaction.
6. *DidRecover*: Executes a [DID-Recover](../CRUD-operations/did-recover.md) tyronZIL transaction.
7. *DidUpdate*: Executes a [DID-Update](../CRUD-operations/did-update.md) tyronZIL transaction.
8. *DidDeactivate*: Executes a [DID-Deactivate](../CRUD-operations/did-deactivate.md) tyronZIL transaction.

### The xWallet transitions

9. *SsiToken*: To set/update a SSI Token.
10. *SsiTokenCallBack*: The [init.tyron](./init.tyron.md) contract calls this transition to update a SSI Token to its latest implementation (token name, proxy address and token implementation address).
11. *ZilIn*: Adds native funds (ZIL) to the xWallet.
12. *ZilOut*: Sends ZIL from the xWallet to any recipient that implements the input tag (e.g. "ZilIn", "").
13. RecipientAcceptTransfer: The ZRC-2 acceptance transition - MUST be defined or transfers to this xWallet will fail otherwise.
14. *XTransfer*: Transfers ZRC-2 SSI Tokens.
15. *TransferSuccessCallBack*: the ZRC-2 callback transition - MUST be defined or transfers from this xWallet will fail otherwise.
16. *Donate*: To initialize a donation campaign code.
17. *DonateCallBack*: The [init.tyron](./init.tyron.md) contract calls this transition to save the donation campaign code into the DIDC.
