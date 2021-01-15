![tyronZIL-logo](./tyronzil-logo.png){: width="600" loading=lazy}

# tyronZIL: The tyron DID Method v2.0.0

Zilliqa's W3C Decentralized Identifier Method

## Problem summary

Identities on the internet remain centralized, mainly by identity providers such as Facebook, Google and PayPal. Thus, when people shop online and login with these accounts, they don't have enough control nor understanding of how their data is used and shared with third parties. Furthermore, decentralized applications are still difficult to grasp for most regular users, and Decentralized Identifiers (DIDs) can make the user experience much better while increasing privacy and security.

## Tyron

Self-Sovereign Identity (SSI) allows people to manage their digital identities, proving who they are without a middleman, by anchoring DIDs on blockchain platforms as a shared root of trust. However, most blockchains still can't provide decentralized identity at scale. By implementing the Tyron SSI Protocol, tyronZIL aims to solve this issue and enable user-controlled digital identities.

The word 'tyron' derives from the Greek *turannos* that means sovereign, and **the purpose of tyron is to give people sovereignty over their data**.

## Conformance

The tyronZIL DID Method specification is conformant with the World Wide Web Consortium (W3C) [Decentralized Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/) specification and the first DID Method for the Zilliqa blockchain platform registered in the [DID Specification Registries](https://w3c.github.io/did-spec-registries/).

- [Intro to W3C DIDs](./W3C-dids.md)

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this specification are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

Versions get organized by [Semantic Versioning](https://semver.org/).

## DID Method

The tyronZIL DID Method is part of the open-source Tyron SSI Protocol that defines its:

- Scheme:
      - [DID Scheme](./scheme/did-scheme.md)
      - [DID-URL syntax](./scheme/did-url-syntax.md)

- [DID Document](./did-document.md)

- DID CRUD operations:
      - [Create](./CRUD-operations/did-create.md)
      - [Resolve](./CRUD-operations/did-resolve.md)
      - [Recover](./CRUD-operations/did-recover.md)
      - [Update](./CRUD-operations/did-update.md)
      - [Deactivate](./CRUD-operations/did-deactivate.md)

- [Security & privacy considerations](./security-privacy.md)

## Protocol

The Tyron Self-Sovereign Identity Protocol, based on smart-contract technology to solve the issue of DID scalability, describes the [DID Method](#did-method) and the cryptographic information to instantiate, deploy and manage the DID smart contract that has the user as its owner. As an SSI protocol, it is non-custodial - the user is in control of their Decentralized Identifier. The Tyron SSI Protocol's goal is to make the DID smart contract smarter while keeping it as simple as possible.


- [Protocol default parameters](./protocol-parameters.md)
- Smart contracts:
      - [DID.tyron: The Decentralized Identifier Smart Contract](./smart-contracts/didc.md)
      - [init.tyron: The SSI Initialization & DNS Smart Contract](./smart-contracts/init.tyron.md)

Smart contracts on Zilliqa get written in [Scilla](https://scilla-lang.org/) (Smart Contract Intermediate-Level Language) that has a design with a focus on safety, imposing a language structure that makes applications less vulnerable to attacks by eliminating known vulnerabilities directly at the language-level.

> Scilla provides formal verification with embedding into the [Coq proof assistant](https://coq.inria.fr/).

## Implementation

- DID Client: [tyronZIL-js](https://github.com/julio-cabdu/tyronZIL-js) is the open-source reference implementation for Node.js, written in TypeScript
- [Models](./implementation/models.md)

## Development

- [Roadmap](./roadmap.md)
