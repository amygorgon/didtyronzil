![tyronZIL-logo](./tyronzil-logo.png){: width="400" loading=lazy}

# tyronZIL DID-Method specification v0.4

A W3C DID-Method powered by [Zilliqa](https://zilliqa.com)

Developed by [Julio Cabrapan Duarte](https://github.com/julio-cabdu)

Funded by [ZILHive](https://zilhive.org/)

## Conformance

The tyronZIL DID-Method is conformant with the World Wide Web Consortium (W3C) [Decentralized Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/) specification.

TyronZIL implements [Sidetree delta-based DID operations](https://identity.foundation/sidetree/spec/#did-operations) and saves the state in a [Scilla smart-contract](https://scilla-lang.org/) that is owned by the user. The user is both the DID-Subject and DID-Controller of their Decentralized Identifier.

## Problem summary

Identities on the internet remain centralized, mainly by identity providers such as Facebook, Google and PayPal. Thus, when people shop online and login with these accounts, they don't have enough control nor understanding of how their data is used and shared with third parties.

Self-Sovereign Identity (SSI) allows people to manage their digital identities, proving who they are without a middleman, by anchoring Decentralized Identifiers (DIDs) on blockchain platforms/distributed-ledgers as a shared root of trust. However, most DLTs still can't provide decentralized identity at scale. By implementing Decentralized Identifiers powered by Sidetree & Scilla, tyronZIL aims to solve this issue to enable user-controlled digital identities.

## Index

- [W3C DIDs](./W3C-dids.md)
- [Sidetree protocol and default parameters](./sidetree.md)
- [DID-Document](./did-document.md)
- Scheme:
      - [DID-Scheme](./scheme/did-scheme.md)
      - [DID-URL syntax](./scheme/did-url-syntax.md)
- Operations:
      - [Introduction](./operations/tyronZIL-operations.md)
      - DID CRUD:
        - [Create](./operations/CRUD/did-create.md)
        - [Resolve](./operations/CRUD/did-resolve.md)
        - [Recover](./operations/CRUD/did-recover.md)
        - [Update](./operations/CRUD/did-update.md)
        - [Deactivate](./operations/CRUD/did-deactivate.md)
- Implementation:
      - [tyronZIL-js](https://github.com/julio-cabdu/tyronZIL-js): The open-source reference implementation for Node.js, written in TypeScript
      - [Sidetree models](./implementation/models.md)
- [Privacy Considerations](https://w3c.github.io/did-core/#privacy-considerations)
