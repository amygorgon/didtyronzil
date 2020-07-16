# tyronZIL DID document

> For an introduction, read [this](./W3C-dids.md#did-document)

A DID document is a graph-based data structure, serialized according to a particular syntax.

TyronZIL's serialization format is JSON. (to-do check if sidetree is json-ld)

Core representation in JSON:

- It defines an unambiguous encoding and decoding of all properties and their associated values.
- It is associated with an IANA-registered MIME type.
- It defines fragment processing rules for its MIME type. to-do: what is MIME

- Producers MUST indicate JSON as the media type in the document's metadata. Consumers MUST determine that the "content-type" DID resolver metadata field is **application/did+json**

6.1.1 Production to-do
6.1.2 Consumption

As defined by W3C, a DID document consists of a collection of property-value pairs.

## Core properties

A TyronZIL DID document MUST have the following properties. Unknown properties MUST be ignored.

### id

All W3C DID documents MUST include the "id" property.

The "id" property denotes the DID [subject](./W3C-dids.md#did-subject), and its value MUST be a single valid tyronZIL DID, e.g.:

```json
{
    "id": "did:tyron:zil:91tDAKCERh95uGgKbJNHYp",
    ...
}
```

### verificationMethod

<mark> tyronZIL DID documents MUST include the "verificationMethod" property. </mark>

> Read the verification method definition [here]](./W3C-dids.md#verification-method)

The "verificationMethod" value MUST be an array of verification method objects. The "verificationMethod" property can be, e.g. a "publicKey" property: to-do: check sidetree requirement

```json
{
    "id": "did:tyron:zil:91tDAKCERh95uGgKbJNHYp",
    to-do: add example
    ...
}
```

Each verification method object MUST have the following properties:

- "id": Its value MUST be a unique DID URL. There MUST NOT be multiple verification method objects with the same id-value - otherwise the DID document processor MUST produce an error. to-do: test sidetree processor
- "type": Its value MUST be exactly one verification method type. to-do: check sidetree key type.
- "controller": Its value MUST be a valid tyronZIL DID. If the verification method is a public key, then the controller property identifies the DID that controls the corresponding private key. to-do: can tyron have a controller property pointing to a different self-owned DID as a backup/recovery - see sidetree recovery.
- Specific verification method properties.

5.4 Public keys
5.5 Authentication to-do
5.6 Authorization and delegation > maybe for under age subjects.

## Sidetree implementation

### Public key

/sidetree/lib/core/versions/latest/models/PublicKeyModel.ts

```js
/**
 * Data model representing a public key in the 'publicKey' array in patches.
 */
export default interface PublicKeyModel {
  id: string;
  type: string;
  jwk: any;
  purpose: PublicKeyPurpose[];
}
```

> The purpose property is an array with elements of type PublicKeyPurpose.

The PublicKeyPurpose enum has two members: Auth and General, both constant-initialized with a string literal ("auth" and "general" respectively).

to-do: why?

/sidetree/lib/core/versions/latest/PublicKeyPurpose.ts

```js
/**
 * Sidetree public key purpose.
 */
enum PublicKeyPurpose {
  Auth = 'auth',
  General = 'general'
}

export default PublicKeyPurpose;
```

### DocumentComposer

to-do: explore

import DocumentComposer from '../../lib/core/versions/latest/DocumentComposer';
