![tyronZIL-logo](./tyronzil-logo.png){: width="600" loading=lazy}

# Decentralized Identifier Method & Self-Sovereign Identity Protocol v2.0.0

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

## Decentralized Identifier Method

This DID Method is part of the open-source Tyron SSI Protocol that defines its:

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

## Self-Sovereign Identity Protocol

The Tyron SSI Protocol solves the DID scalability issue by implementing the .tyron smart contracts. The protocol provides the cryptographic information to instantiate a Self-Sovereign Identity (ssi.tyron smart contract) which manages upgradable versions of the Decentralized Identifier (did.tyron smart contract). The user is the contract owner. The protocol is non-custodial - the user must show control of their Zilliqa private key for the Self-Sovereign Identity to process their transaction.

- [Protocol default parameters](./protocol-parameters.md)
- Smart-contract implementations:
      - [DID.tyron: The Decentralized Identifier Smart Contract](./smart-contracts/didc.md)
      - [init.tyron: The SSI Initialization & DNS Smart Contract](./smart-contracts/init.tyron.md)

Smart contracts on Zilliqa get written in [Scilla](https://scilla-lang.org/) (Smart Contract Intermediate-Level Language) that has a design with a focus on safety, imposing a language structure that makes applications less vulnerable to attacks by eliminating known vulnerabilities directly at the language-level.

> Scilla provides formal verification with embedding into the [Coq proof assistant](https://coq.inria.fr/).

## Off-chain software

- DID Client: [tyronZIL-js](https://github.com/julio-cabdu/tyronZIL-js) for Node.js, written in TypeScript
- Software Development Kit: [tyronzil-sdk](https://github.com/pungtas/tyronzil-sdk) for React Native
- [Models](./implementation/models.md)

## Development

- [Roadmap](./roadmap.md)
