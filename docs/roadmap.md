# Development roadmap

The project got organized in the following development sprints:

Dates | Sprint
---|---
29/6 - 24/7/2020 | Created the [tyronzil Node.js client](https://www.github.com/tralcanx/tyronzil) that performs the CRUD operations (create, read, recover, update & deactivate).
27/7 - 21/8/2020 | Integrated the SSI client with the Zilliqa blockchain platform.
24/8 - 18/9/2020 | Implemented the [Decentralized Identifier smart contract](https://www.tyronzil.com/smart-contracts/DID.tyron/) according to the tyronzil W3C DID Method.
21/9 - 11/12/2020 | **Tyron Improvement Proposal #1**: Three sprints to increase security and discoverability of Tyron Decentralized Identifiers.
21/9 - 16/10/2020 | TIP1.1 implemented Zilliqa's 32-byte private-keys to generate Schnorr signatures that can be verified by the DID smart contract. Introduced the [DID Keys](https://www.tyronzil.com/protocol-parameters/#did-keys) that have their public keys stored in the DID contract. [Release notes](https://github.com/tralcanx/tyronzil/releases/tag/v1.0.0-alpha).
19/10 - 13/11/2020 | TIP1.2 developed the [INIT.tyron smart contract](https://www.tyronzil.com/smart-contracts/INIT.tyron/) for the initialization of self-sovereign identities as well as a domain name system that implements the *.did*, *.agent* & *.tyron* domains. These top-level SSI Domains define the SSI Trinity constituted by the user as the owner of their Self-Sovereign Identity, the agent providing the application, and tyron as the technology & open association. This development sprint also implemented the xWallet that gives ZRC-2 transfer capabilities to the DID smart contract. Furthermore, TIP1.2 developed a prototype for donations named [pung.me](https://www.pung.me). [Release notes](https://github.com/tralcanx/tyronzil/releases/tag/v2.0.0-alpha).
16/11 - 11/12/2020 | TIP1.3 developed the [tyron.js JavaScript library](https://github.com/pungtas/tyron.js) to be utilized by the tyronzil client & newly implemented [DID Browser](https://github.com/pungtas/tyron-react-native) React Native application prototype - check out the demo [here](https://www.youtube.com/watch?v=9aiKGehR_gU).