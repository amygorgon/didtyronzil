**<p style="text-align: center;">
![tyron](./tyron.png){: width="500" loading="lazy"}
</p>**

```own your data, empower your world```

A Self-Sovereign Identity (SSI) protocol on blockchain technologies.

The user account on Tyron is its own immutable smart contract code (the SSI.tyron) which can deploy diverse and privacy-preserving Decentralized Identifier (DID) dapps to interact with other smart contracts and platforms. 

The Tyron DID Method is part of the [W3C DID Specification Registries](https://w3c.github.io/did-spec-registries/) and the first DID Method for the [Zilliqa](https://www.zilliqa.com) blockchain platform, funded by [ZILHive](https://www.zilhive.org) Innovation track.

SSI allows people to manage their digital identities, proving who they are without a middleman, by anchoring DIDs on decentralized networks as a shared root of trust. With smart-contract technology, the Tyron SSI Protocol aims to provide decentralized identity at scale and enable user-controlled self-sovereign  identities.

The meaning of the word 'Tyron' derives from the Greek *turannos* that means sovereign, and **the purpose of this protocol is to give people sovereignty over their data**.

The Tyron SSI Protocol solves the DID scalability issue by implementing the .tyron smart contracts. The protocol provides the code to instantiate a Self-Sovereign Identity (the SSI.tyron smart contract) which manages upgradable versions of Decentralized Identifier smart contracts.

The protocol is non-custodial - the user must show control of their Zilliqa private key for the Self-Sovereign Identity to process their transaction. The user is the owner of the SSI and the SSI the owner of DID dapps.

- [Protocol default parameters](./protocol-parameters.md)
- Smart-contract implementations:
      - SSI.tyron: Self-Sovereign Identity smart contract (in development)
      - [DID.tyron: Decentralized Identifier smart contract](./smart-contracts/DID.tyron.md)
      - [INIT.tyron: SSI Initialization & DNS smart contract](./smart-contracts/INIT.tyron.md)

Smart contracts on Zilliqa get written in [Scilla](https://scilla-lang.org/) (Smart Contract Intermediate-Level Language) that has a design with a focus on safety, imposing a language structure that makes applications less vulnerable to attacks by eliminating known vulnerabilities directly at the language-level.

> Scilla provides formal verification with embedding into the [Coq proof assistant](https://coq.inria.fr/).

## The Tyron Decentralized Identifier Method v2.0.0

Identities on the internet remain centralized, mainly by identity providers such as Facebook, Google and PayPal. Thus, when people shop online and login with these accounts, they don't have enough control nor understanding of how their data is used and shared with third parties. Furthermore, decentralized applications are still difficult to grasp for most regular users, and Decentralized Identifiers (DIDs) can make the user experience much better while increasing privacy and security.

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

## Conformance

This DID Method specification is conformant with the World Wide Web Consortium (W3C) [Decentralized Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/) specification.

- [W3C DIDs terminology](./W3C-dids.md)

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this specification are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

Versions get organized by [Semantic Versioning](https://semver.org/).

## Off-chain software

- SSI client: [tyronzil](https://github.com/tralcanx/tyronzil) for Node.js
- SSI library: [tyronzil-js](https://github.com/pungtas/tyronzil-js)
- React Native application: [tyron](https://github.com/pungtas/tyron)
- [Models](./implementation/models.md)

## Development

- [Roadmap](./roadmap.md)
