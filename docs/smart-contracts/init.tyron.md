# init.tyron: The SSI Initialization & Domain Name System Smart Contract

Self-sovereign identity (SSI) focuses on principle #7 of Privacy by Design: "Respect for user privacy — keep it user-centric". In that spirit, the user is the owner of their DID smart contract (DIDC) and MUST be the caller to every [transition](./didc.md#transitions) that affects the DIDC state.

On the other hand, the user relies on servers and cloud infrastructure that is provided by the SSI agents (e.g., pungtas.agent).

Tyron Pungtas is the open organization developing tyron as a foundation. Decentralization and tertiarization of services aim at being integrated by regionally diverse SSI agents. In future tyron improvement proposals, there will be an agent.tyron smart contract developed.

The release of the DID-Client tyronZIL v2.0 introduces the concept of the SSI Trinity integrated by the domain name systems user.did, avatar.agent and contract.tyron - it gets defined by the DID smart contract (DIDC) as follows:

```
(* The SSI Trinity ADT *)
(*---------------------*)
  type SsiTrinity =
    | User              (* the contract owner *)
    | Agent of String   (* the agent is the entity running the DID-Client)
    | Tyron             (* the .tyron contracts *)
```

> Find the [init.tyron's code](https://github.com/julio-cabdu/tyronZIL-js/blob/master/src/lib/blockchain/smart-contracts/init.tyron.scilla) on GitHub.

## Immutable fields

Immutable fields are set at deployment and can not get modified. The DIDC gets deployed with the following immutable fields:

0. *initFoundationAddress*: The Zilliqa adddress of the Tyron Pungtas Foundation, the owner of the init.tyron contract | [ByStr20 type]

## Mutable fields

Mutable fields can get modified but only if there is a specific code in the contract that allows it. They MUST get initialized with a value at deployment.

0. *foundation_addr*: The Zilliqa address of the Tyron Pungtas Foundation that can get updated by the *UpdateOwner* transition | [ByStr20 type]
1. *didc_code*: The init.tyron stores the DIDC code by version. This way, the agents can download the DIDC code by version directly from the Zilliqa blockchain | @key: version | @value: hex-encoded code | [Map String String type]
2. *operation_cost*: The cost of a tyronZIL transaction | @key: domain OR contract.tyron | @value: cost as % of the amount | [Map String Uint128 type]
3. *agent_commission*: The agent's commission as a % of share (the user has 51% of the share, Tyron Pungtas Foundation 51% of the remaining 49%, and the agent earns the rest and can offer promotions) | @key: avatar.agent | @value: max 24% | [Map String Uint128 type]
4. *dns*: The address of a user's DIDC gets stored in the DNS records map field defined as follows:
```
   @key: SsiDomain (e.g. ".did", ".tyron", ".agent")
   @value: Map of
            @key: avatar
            @value: address *)
  field dns: Map String (Map String ByStr20) = Emp String (Map String ByStr20)
```
5. *ssi_tokens*: The proxy addresses of the SSI Tokens | @key: token | @value: proxy address | [Map String ByStr20 type]
6. *ssi_token_implementations*: The addresses of the SSI Token implementations | @key: proxy address | @value: implementation address | [Map ByStr20 ByStr20 type]

## Procedures 

Procedures can change the state of the contract (mutable fields), but they are not part of the public API which means that they can only get invoked from within the contract itself.

0. *ThrowError*: The procedure to throw an error.
1. *Payment*: Executes the payment to the foundation.
2. *IsRightCaller*: Validates the _sender.

## Transitions

Transitions are the public API of the DIDC and get invoked by sending messages to the contract.  

0. *UpdateOwner*: Updates the Zilliqa address of the Tyron Pungtas Foundation.
1. *SetDidCode*: Saves the DID-Smart-Contract code by version.
2. *OperationCost*: Sets the *operation_cost* by SSI domain (domain name or donation campaign code).
3. *AgentCommission*: Sets the *agent_commission* by domain name.agent of type String.
4. *Init*: Called by a DIDC to set the .did operation cost, foundation address & agent's account.
5. *SetSsiDomain*: Sets a Self-Sovereign Identity domain name in the DNS records.
6. *SetSsiToken*: Sets the token name, proxy address and implementation address of a SSI Token.
7. *SsiToken*: Called by a DIDC to initialize a SSI Token in its state.
8. *Donate*: Called by a DIDC to initialize a donation campaign code in its state.
9. *DeleteCampaign*: Removes a donation campaign code from the *operation_cost* map field.
