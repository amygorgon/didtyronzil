**<p style="text-align: left;">
![tyron](./tyronzil.png){: width="505" loading="lazy"}
</p>**

## Tyron SSI Protocol's Decentralized Identifier Method Specification

Tyronzil is the W3C Decentralized Identifier Method of the [Tyron Self-Sovereign Identity Protocol](https://www.ssiprotocol.com). You can find it published at the [W3C DID Specification Registries](https://w3c.github.io/did-spec-registries/), and it is the first DID Method of the [Zilliqa](https://www.zilliqa.com) blockchain - funded by [ZILHive](https://www.zilhive.org) Innovation grants.

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

- [Parameters](./protocol-parameters.md)

- [Models](./implementation/models.md)

The tyronzil W3C DID Method implementation is the [did.tyron](https://github.com/pungtas/smart-contracts.tyron/blob/main/DID/did.tyron.scilla) smart contract.

Smart contracts on Zilliqa get written in [Scilla](https://scilla-lang.org/) (Smart Contract Intermediate-Level Language) that has a design focused on safety, imposing a language structure that makes applications less vulnerable to attacks by eliminating known vulnerabilities directly at the language level. Scilla provides formal verification with embedding into the [Coq proof assistant](https://coq.inria.fr/).

## Conformance

The tyronzil DID Method specification is conformant with the World Wide Web Consortium (W3C) [Decentralized Identifiers (DIDs) v1.0](https://w3c.github.io/did-core/) specification.

- [W3C DIDs terminology](./W3C-dids.md)

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this specification are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

Versions get organized by [Semantic Versioning](https://semver.org/).