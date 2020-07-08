# tyronZIL DID method specification to-do add version

A W3C Sidetree-based DID method for the Zilliqa blockchain

Funded by [ZILHive](https://zilhive.org/)

Developed by [Julio Cabrapan Duarte](https://github.com/julio-cabdu)

Conformant to the requirements specified in the:

i. World Wide Web Consortium (W3C) DID specification: [Decentralized
Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/)

ii. Decentralized Identity Foundation (DIF) [Sidetree protocol specification](https://identity.foundation/sidetree/spec/)

## Problem summary

Identities on the internet remain centralized, mainly by identity providers such as Facebook and Google. Thus, when people shop online and login with these accounts, they don't have enough control nor understanding of how their data is used and shared with third parties.

Self-sovereign identity (SSI) allows people to manage their digital identities and prove who they are without a centralized authority, by anchoring decentralized identifiers (DIDs) on blockchain platforms. However, most DLTs still can't provide SSI applications at scale. By implementing the Sidetree protocol on top of Zilliqa, tyronZIL aims to solve this issue and enable user-controlled digital identities in the Zilliqa ecosystem.

## Table of contents

- [W3C DIDs terminology](./W3C-terminology.md)
- [Sidetree protocol terminology and default parameters in tyronZIL](./Sidetree-terminology.md)
- [tyronZIL DID scheme](./DID-scheme.md)
- [tyronZIL DID document](./DID-document.md)
- [tyronZIL operations](./tyronZIL-operations/method-operations.md)
    - [DID resolution](./tyronZIL-operations/DID-resolution.md)
- [Security and privacy considerations](./to-do)
