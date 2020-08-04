# tyronZIL DID document

> For an introduction, read [this](./W3C-dids.md#did-document)

A DID-document is a graph-based data structure, a collection of property-value pairs, serialized according to a particular syntax.

TyronZIL's serialization format is JSON:  

- It defines an unambiguous encoding and decoding of all properties and their associated values
- It MUST be a single JSON object conforming to [RFC8259, The JavaScript Object Notation (JSON) Data Interchange Format](https://tools.ietf.org/html/rfc8259)  
- The names of the members of the JSON object MUST correspond to the [core property names](#core-properties) of the DID-document  
- Property values MUST be:  
        - [Numbers](https://tools.ietf.org/html/rfc8259#section-6) for number values representable as IEEE754  
        - [Literal values](https://tools.ietf.org/html/rfc8259#section-3) for boolean ('false', 'true') and empty values ('null')  
        - [Arrays](https://tools.ietf.org/html/rfc8259#section-5) for sequence values and unordered sets of values  
                - [Objects](https://tools.ietf.org/html/rfc8259#section-4) for sets of properties  
                - [Strings](https://tools.ietf.org/html/rfc8259#section-7) for all other values. Consumers MAY further parse these strings into more specific data types such as URIs and date stamps  
- The 'contentType' property in the resolver's metadata MUST be ```application/did+json```:  
        - Which is the associated IANA-registered MIME type, with its corresponding rules to process the fragment  
        - [Producers](./W3C-dids.md#producer) MUST write this type in the document's metadata, and [consumers](./W3C-dids.md#consumer) MUST validate it as well
- Consumers MUST ignore unknown object member names as unknown properties

## Core properties

### id

The ```id``` value MUST be a single valid tyronZIL DID itself, e.g.:

```json
{
  "id": "did:tyron:zil:test:EiBtH2NHC5nOdcp6iMTjq2rvuQj5gbvnSwgqYIMMXne38w"
}
```

All W3C DID documents MUST include the "id" property, which denotes the [DID subject](./W3C-dids.md#did-subject).

### publicKey

The ```publicKey``` property is the main [verification method](./W3C-dids.md#verification-method).

The ```publicKey``` value MUST be an array of objects of type [VerificationMethodModel](./implementation/models.md#verification-method-model), e.g.:

```json
{
  "id": "did:tyron:zil:test:EiBtH2NHC5nOdcp6iMTjq2rvuQj5gbvnSwgqYIMMXne38w",
  "publicKey": [
    {
      "id": "did:tyron:zil:test:EiBtH2NHC5nOdcp6iMTjq2rvuQj5gbvnSwgqYIMMXne38w#primarySigningKey",
      "type": "EcdsaSecp256k1VerificationKey2019",
      "jwk": {
        "kty": "EC",
        "crv": "secp256k1",
        "x": "ksEhVIcb7JGKp_zNL0NJJxFBNzGgERzSQrZjEfkvPJc",
        "y": "J3h3PgSqdUXkDt1CIZHbWmKvhlD1bedJX6VE3u1o7bE",
        "kid": "G0cATxTiiCC4Xt3NnANRCrfBOexYpQhB0Sy616E5LTE"
      }
    },
    {
      "id": "did:tyron:zil:test:EiBtH2NHC5nOdcp6iMTjq2rvuQj5gbvnSwgqYIMMXne38w#anotherSigningKey",
      "type": "EcdsaSecp256k1VerificationKey2019",
      "publicKeyJwk": {
        "kty": "EC",
        "crv": "secp256k1",
        "x": "ynsq5eFLNaV-rIJxpT_QMBZ3dXv2kQwHzw7VZKjU8fs",
        "y": "q_Nw9PhUkWpt0mV5C62qydBokP09p8gkSKy9nFNTq60",
        "kid": "ofnZh1sI0we7eYfiDfIxcqK10NezOLvcQfCEBacbzOQ"
      }
    }
  ]
}
```

---

All verification methods MUST have the following properties:

- "id": Its value MUST be a unique tyronZIL DID URL. There MUST NOT be multiple verification method objects with the same id-value - otherwise the [consumer](./W3C-dids.md#consumer) MUST produce an error
- "type": Its value MUST be exactly one verification method type. The default type is currently ```EcdsaSecp256k1VerificationKey2019```
- "controller": Its value MUST be a valid tyronZIL DID representing the entity that controls the corresponding private key
- "publicKeyJwk": The cryptographic key itself expressed as a [IETF RFC 7517](https://tools.ietf.org/html/rfc7517) compliant JSON Web Key (JWK) representation for the [KEY_ALGORITHM](./sidetree.md#key-algorithm)

Before processing them into the DID-document, each verification method has a property called ```purpose```. It states the functionality of the key, its [verification relationship](./W3C-dids.md#verification-relationship). For public keys, the purpose value MUST be an array of [PublicKeyPurpose variants](./implementation/models.md#public-key-purpose).

---

### authentication

At least one public key MUST have its purpose as ```PublicKeyPurpose.Auth = 'auth'```.

The ```authentication``` value MUST be an array of verification methods. Each verification method MAY be embedded or referenced, e.g.:

```json
{
  "authentication": [
    // referenced key:
    "did:tyron:zil:test:EiBtH2NHC5nOdcp6iMTjq2rvuQj5gbvnSwgqYIMMXne38w#primarySigningKey",
    // embedded key:
    {
      "id": "did:tyron:zil:test:EiBtH2NHC5nOdcp6iMTjq2rvuQj5gbvnSwgqYIMMXne38w#authentication-key",
      "type": "EcdsaSecp256k1VerificationKey2019",
      "jwk": {
        "kty": "EC",
        "crv": "secp256k1",
        "x": "W5-Wa5xk6rtIyC4b0cp3JAb8-Rmhc_CIRPt-JPexZWY",
        "y": "XD15geXy_UBRByhnI_fuAZhvWSsaYR0L92jTLr63xk8",
        "kid": "Juvzhd0beV-bR8Oq12JYt4wyYYZ8Zndrb9oM_WMxoF4"
      }
    }
  ]
}
```

### Service endpoints

tyronZIL service endpoints are used to express ways of communicating with the [DID subject](./W3C-dids.md#did-subject), from privacy preserving messaging services to cryptocurrency addresses.

The ```service``` value MUST be an array of objects of type [ServiceEndpointModel](./implementation/models.md#service-endpoint-model), e.g.:

```json
{
  "service": [
    {
      "id": "did:tyron:zil:test:EiBtH2NHC5nOdcp6iMTjq2rvuQj5gbvnSwgqYIMMXne38w#tyronZIL-website",
      "type": "method-specification",
      "endpoint": "https://tyronZIL.com"
    },
    {
      "id": "did:tyron:zil:test:EiBtH2NHC5nOdcp6iMTjq2rvuQj5gbvnSwgqYIMMXne38w#ZIL-address",
      "type": "cryptocurrency-address",
      "endpoint": "zil1egvj6ketfydy48uqzu8qphhj5w4xrkratv85ht"
    }
  ]
}
```

---

All services MUST have the following properties:

- "id": Its value MUST be a unique tyronZIL DID URL with a length no more than fifty (50) ASCII encoded characters. There MUST NOT be multiple service objects with the same id-value - otherwise the [consumer](./W3C-dids.md#consumer) MUST produce an error
- "type": Its value MUST be a string with a length of no more than thirty (30) ASCII encoded characters
- "endpoint": Its value MUST be a valid URI string (including a scheme segment: i.e. http://, git://) OR a cryptocurrency address, with a length of no more than one hundred (100) ASCII encoded characters

If any of the values exceed the specified lengths, the [consumer](./W3C-dids.md#consumer) MUST produce an error.
