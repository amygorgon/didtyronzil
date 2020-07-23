# tyronZIL DID method specification

A W3C Sidetree-based DID method for the [Zilliqa blockchain platform](https://zilliqa.com)

Developed by [Julio Cabrapan Duarte](https://github.com/julio-cabdu)

Funded by [ZILHive](https://zilhive.org/)

## Conformance

The tyronZIL DID method is conformant with the following specifications:

i. World Wide Web Consortium (W3C) [Decentralized
Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/)

ii. Decentralized Identity Foundation (DIF) [Sidetree protocol](https://identity.foundation/sidetree/spec/)

## Problem summary

Identities on the internet remain centralized, mainly by identity providers such as Facebook, Google and PayPal. Thus, when people shop online and login with these accounts, they don't have enough control nor understanding of how their data is used and shared with third parties.

Self-sovereign identity (SSI) allows people to manage their digital identities and prove who they are without a centralized authority, by anchoring decentralized identifiers (DIDs) on blockchain platforms. However, most DLTs still can't provide SSI applications at scale. By implementing the Sidetree protocol on top of Zilliqa, tyronZIL aims to solve this issue and enable user-controlled digital identities in the Zilliqa ecosystem.

## Index

- [W3C DIDs](./W3C-dids.md)
- [Sidetree protocol and default parameters](./sidetree.md)
- [DID-document](./did-document.md)
- Scheme:
      - [DID-scheme](./scheme/did-scheme.md)
      - [DID URL syntax](./scheme/did-url-syntax.md)
- Operations:
      - [Introduction](./operations/tyronZIL-operations.md)
      - DID CRUD:
        - [Create](./operations/CRUD/did-create.md)
        - [Resolve](./operations/CRUD/did-resolve.md)
        - [Recover](./operations/CRUD/did-recover.md)
        - [Update](./operations/CRUD/did-update.md)
        - [Deactivate](./operations/CRUD/did-deactivate.md)
- Implementation: [tyronZIL-js](https://github.com/julio-cabdu/tyronZIL-js) for Node.js
      - [tyronZIL-js Sidetree models](./implementation/models.md)