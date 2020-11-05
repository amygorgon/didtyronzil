# init.tyron: The SSI Initialization & Domain Name System Smart Contract

Self-sovereing identity (SSI) focuses on principle #7 of Privacy by Design: "Respect for user privacy — keep it user-centric". In that spirit, the user is the owner of their DID smart contract (DIDC) and MUST be the caller to every [transition](./didc.md#transitions) that affects the DIDC state.

On the other hand, the user relies on servers and cloud infrastructure that is provided by the SSI agents (e.g., pungtas.agent).

Tyron Pungtas is the open organization developing tyron as a foundation. Decentralization and tertiarization of services aim at being integrated by regionally diverse SSI agents. In future tyron improvement proposals, there will be an agent.tyron smart contract developed.

The release of the DID-Client tyronZIL v2.0 introduces the concept of the SSI Trinity integrated by the domain name systems user.did, avatar.agent and contract.tyron - it gets defined by the DID smart contract (DIDC):

```
(* The SSI Trinity ADT *)
(*---------------------*)
  type SsiTrinity =
    | User              (* the contract owner *)
    | Agent of String   (* the agent is the entity running the DID-Client)
    | Tyron             (* the init.tyron contract *)
```

The address of the user's DIDC gets stored in the DNS resource records mutable field defined as follows:

```
(* The DNS resource records
   @key: SsiDomain (e.g. ".did", ".tyron", ".agent")
   @value: Map of
            @key: avatar
            @value: address *)
  field dns: Map String (Map String ByStr20) = Emp String (Map String ByStr20)
```

The init.tyron stores the DIDC code in its didc_code mutable field:

```
(* The DID-Smart-Contract code by version
   @key: version
   @value: hex-encoded code *)
  field didc_code: Map String String = Emp String String
```

This way, the agents can download the DIDC code by version directly from the Zilliqa blockchain.