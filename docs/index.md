![tyronZIL-logo](./tyronzil-logo.png){: width="400" loading=lazy}

# Tyron's DID-Method specification v1.0.0

***tyronZIL*** is a W3C DID-Method powered by [Zilliqa](https://zilliqa.com)

Developed by [Julio Cabrapan Duarte](https://github.com/julio-cabdu)

Funded by [ZILHive](https://zilhive.org/)

## Problem summary

Identities on the internet remain centralized, mainly by identity providers such as Facebook, Google and PayPal. Thus, when people shop online and login with these accounts, they don't have enough control nor understanding of how their data is used and shared with third parties. Furthermore, decentralized applications are still difficult to grasp for most regular users, and Decentralized Identifiers (DIDs) can make the user experience much better while increasing privacy and security.

## Tyron

Self-Sovereign Identity (SSI) allows people to manage their digital identities, proving who they are without a middleman, by anchoring DIDs on blockchain platforms as a shared root of trust. However, most blockchains still can't provide decentralized identity at scale. By implementing the Tyron SSI Protocol, tyronZIL aims to solve this issue and enable user-controlled digital identities.

The word Tyron derives from the Greek *turannos* that means sovereign, and ***Tyron's purpose is to give people sovereignty over their data***.

## Conformance

The tyronZIL DID-Method is conformant with the World Wide Web Consortium (W3C) [Decentralized Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/) specification and the first DID-Method for the Zilliqa blockchain platform registered in the [DID Specification Registries](https://w3c.github.io/did-spec-registries/).

- [Intro to W3C DIDs](./W3C-dids.md)

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this specification are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

Versions get organized by [Semantic Versioning](https://semver.org/).

## DID-Method

The tyronZIL DID-Method is part of the open-source Tyron SSI Protocol that defines its:

- Scheme:
      - [DID-Scheme](./scheme/did-scheme.md)
      - [DID-URL syntax](./scheme/did-url-syntax.md)

- [DID-Document](./did-document.md)

- CRUD operations:
      - [DID-Create](./CRUD-operations/did-create.md)
      - [DID-Resolve](./CRUD-operations/did-resolve.md)
      - [DID-Recover](./CRUD-operations/did-recover.md)
      - [DID-Update](./CRUD-operations/did-update.md)
      - [DID-Deactivate](./CRUD-operations/did-deactivate.md)

- [Security & privacy considerations](./security-privacy.md)


## Protocol

The Tyron Self-Sovereign Identity Protocol, based on smart-contract technology to solve the issue of DID scalability, describes the [DID-Method](#did-method) and the cryptographic information to instantiate, deploy and manage the DID-Smart-Contract that has the user as its owner. As an SSI protocol, it is non-custodial - the user is in control of their Decentralized Identifier.

- [Protocol default parameters](./protocol-parameters.md)
- Smart contracts (SCs):
      - [Decentralized Identifier SC](./smart-contracts/DID-SC.md)
      - [init.tyron SC](./smart-contracts/init.tyron.md)

Smart contracts on Zilliqa get written in Scilla (Smart Contract Intermediate-Level Language) that has a design with a focus on safety, imposing a language structure that makes applications less vulnerable to attacks by eliminating known vulnerabilities directly at the language-level.

> Scilla provides formal verification with embedding into the [Coq proof assistant](https://coq.inria.fr/).

## Implementation

- DID-Client: [tyronZIL-js](https://github.com/julio-cabdu/tyronZIL-js) is the open-source reference implementation for Node.js, written in TypeScript
- [Models](./implementation/models.md)

## Development

- [Roadmap](./roadmap.md)