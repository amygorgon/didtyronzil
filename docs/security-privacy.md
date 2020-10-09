# Security & privacy considerations

TyronZIL DIDs operate on Zilliqa, a public blockchain platform that implements PBFT (practical byzantine fault tolerance) as the consensus protocol, as explained in the [Zilliqa's whitepaper](https://docs.zilliqa.com/whitepaper.pdf). Given the 'public' nature of the network, Tyron anticipates that messages could be read, or corrupted in case of chain-reorganization. However, as long as there is no 51% attack the ledger's immutability, on which DIDs rely on, remains uncompromised.

The tyronZIL DID-Client currently interacts with Zilliqa nodes hosted by Zilliqa Research Pte. Ltd. as can be seen in the [open-source code](https://github.com/julio-cabdu/tyronzil-js). It is also possible to submit transactions to any other node.

To interact with the user's [DID-Smart-Contract(DID-SC)](./smart-contracts/DID-SC.md), the client MUST submit a blockchain transaction. All Zilliqa transactions require an increasing nonce, mitigating this way replay attacks. The user can check their DID-SC-State on, e.g. [Devex](https://devex.zilliqa.com/?network=https%3A%2F%2Fapi.zilliqa.com) to confirm that their operation did not get delayed. Furthermore, timestamps are supported and coded into the DID-SC.

### Smart contract security

[Scilla](https://learnscilla.com/home) implements smart contract safety at the language level with formal verification.  It utilizes the [Coq Proof Assistant](https://coq.inria.fr/) for mechanized proofs about programs' properties.

### Key revocation, DID recovery & deactivation

If a key is compromised, it is possible to remove it through a [DID-Update](./CRUD-operations/did-update.md) operation with a 'RemoveKeys' patch-action. To perform a DID operation, the user MUST posess the private ***did_update_key*** that corresponds to the public ***did_update_key*** stored in the DID-SC, ensuring that any insertion, deletion or modification happens under stipulated terms.

If the private ***did_update_key*** is compromised, the user can request a [DID-Recover](./CRUD-operations/did-recover.md) operation to replace their DID-State, completely.

The [DID-Deactivate](./CRUD-operations/did-deactivate.md) operation is also supported as long as the private ***did_recovery_key*** is not compromised.

## Privacy Considerations

The tyronZIL DID-Method focuses on principle #7 of Privacy by Design: "Respect for user privacy — keep it user-centric". As a consequence, the user as the [DID-Subject](./W3C-dids.md#did-subject) is the sole [DID-Controller](./W3C-dids.md#did-controller) of their Decentralized Identifier.

### Keep Personally-Identifiable Information (PII) private

Given that Zilliqa is a public & decentralized network, personal data MUST NOT get included in the [DID-Document](./did-document.md), ensuring the user's [right to be forgotten](https://en.wikipedia.org/wiki/Right_to_be_forgotten) All personal information MUST exist behind service endpoints, under the user's control.

The exchange of personal data MUST occur on private, peer-to-peer communication channels.

### Correlation risk

The user MUST be aware that if using their Tyron DID with more than one party, then they are implicitly authorizing correlation between those parties. To mitigate this, a user can have as many Decentralized Identifiers as needed to engage in pairwise interactions. Either way, correlation can still occur if the same keys or personal service endpoints get used in different DID-Documents. Unique endpoints allow traffic to be easily correlated, so a better strategy is to share an endpoint among many DIDs. Tyron assumes uncompromised endpoints.
