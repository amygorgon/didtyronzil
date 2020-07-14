# tyronZIL DID method specification

A W3C Sidetree-based DID method for the Zilliqa blockchain platform

Developed by [Julio Cabrapan Duarte](https://github.com/julio-cabdu)

Funded by [ZILHive](https://zilhive.org/)

Conformant to the requirements specified in the:

i. World Wide Web Consortium (W3C) DID specification: [Decentralized
Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/)

ii. Decentralized Identity Foundation (DIF) [Sidetree protocol specification](https://identity.foundation/sidetree/spec/)

## Problem summary

Identities on the internet remain centralized, mainly by identity providers such as Facebook, Google and PayPal. Thus, when people shop online and login with these accounts, they don't have enough control nor understanding of how their data is used and shared with third parties.

Self-sovereign identity (SSI) allows people to manage their digital identities and prove who they are without a centralized authority, by anchoring decentralized identifiers (DIDs) on blockchain platforms. However, most DLTs still can't provide SSI applications at scale. By implementing the Sidetree protocol on top of Zilliqa, tyronZIL aims to solve this issue and enable user-controlled digital identities in the Zilliqa ecosystem.

## Table of contents

- [W3C DIDs](./W3C-dids.md)
- [Sidetree protocol and default parameters](./sidetree.md)
- [tyronZIL DID scheme](./did-scheme.md)
- [tyronZIL DID document](./did-document.md)
- [tyronZIL DID operations](./operations/tyronZIL-operations.md)
    - [Create DID](./operations/CRUD/did-create.md)
    - [Resolve DID](./operations/CRUD/did-resolve.md)
    - [Recover DID](./operations/CRUD/did-recover.md)
    - [Update DID](./operations/CRUD/did-update.md)
    - [Deactivate DID](./operations/CRUD/did-deactivate.md)