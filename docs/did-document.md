# Tyron DID-Document

> For an introduction, read [this](./W3C-dids.md#did-document).

A DID-Document is a graph-based data structure, a collection of property-value pairs, serialized according to a particular syntax.

The Tyron DID-Document's serialization format is **JSON**:  

- It defines an unambiguous encoding and decoding of all properties and their associated values
- It MUST be a single JSON object conforming to [RFC8259, The JavaScript Object Notation (JSON) Data Interchange Format](https://tools.ietf.org/html/rfc8259)  
- The names of the members of the JSON object MUST correspond to the [core property names](#core-properties) of the DID-Document  
- Property values MUST be:  
        - [Numbers](https://tools.ietf.org/html/rfc8259#section-6) for number values representable as IEEE754  
        - [Literal values](https://tools.ietf.org/html/rfc8259#section-3) for boolean ('false', 'true') and empty values ('null')  
        - [Arrays](https://tools.ietf.org/html/rfc8259#section-5) for sequence values and unordered sets of values  
                - [Objects](https://tools.ietf.org/html/rfc8259#section-4) for sets of properties  
                - [Strings](https://tools.ietf.org/html/rfc8259#section-7) for all other values. Consumers MAY further parse these strings into more specific data types such as URIs and date stamps  
- The ***contentType*** property in the resolver's metadata MUST be ```application/did+json```:  
        - Which is the associated IANA-registered MIME type, with its corresponding rules to process the fragment  
        - [Producers](./W3C-dids.md#producer) MUST write this type in the document's metadata, and [consumers](./W3C-dids.md#consumer) MUST validate it as well
- Consumers MUST ignore unknown object member names as unknown properties

## Core properties

### 1. id

The ```id``` value MUST be a single valid Tyron DID itself, e.g.:

```json
{
  "id": "did:tyron:zil:test:0x5a156a1d18a9a76a0a86b62fcdcb2e547173f3c9"
}
```

All W3C DID-Documents MUST include the ```id``` property, which denotes the [DID-Subject](./W3C-dids.md#did-subject).

### 2. publicKey

The ```publicKey``` property is the main [verification method](./W3C-dids.md#verification-method).

The ```publicKey``` value MUST be an array of objects of type [VerificationMethodModel](./implementation/models.md#verification-method-model), e.g.:

```json
{
  "id": "did:tyron:zil:test:0x5a156a1d18a9a76a0a86b62fcdcb2e547173f3c9",
  "publicKey": [
    {
      "id": "did:tyron:zil:test:0x5a156a1d18a9a76a0a86b62fcdcb2e547173f3c9#keyID-1",
      "type": "SchnorrSecp256k1VerificationKey2019",
      "publicKeyBase58": "sPKxpZouU4wtdYsTN6a9nJtkfEYPPebSGVsz9ZWJPU3N"
    },
    {
      "id": "did:tyron:zil:test:0x5a156a1d18a9a76a0a86b62fcdcb2e547173f3c9#keyID-2",
      "type": "SchnorrSecp256k1VerificationKey2019",
      "publicKeyBase58": "tfJRkK8kEu8YFML8bwpkZwzo8YXDFhfFuryfGQAtw4jg"
    }
  ]
}
```

All verification methods MUST have the following properties:

- ***id***: Its value MUST be a unique Tyron DID-URL. There MUST NOT be multiple verification method objects with the same id-value - otherwise the [consumer](./W3C-dids.md#consumer) MUST produce an error
- ***type***: Its value MUST be exactly one verification method type. The Tyron default type is currently ```SchnorrSecp256k1VerificationKey2019```
- ***publicKeyBase58***: the secp256k1 public key value encoded as a raw 32-byte string in [Base58](https://tools.ietf.org/html/draft-msporny-base58-01) Bitcoin format

Before processing them into the DID-Document, each verification method has a property called ```purpose```. It states the functionality of the key, its [verification relationship](./W3C-dids.md#verification-relationship). For public keys, the purpose value MUST be an array of [PublicKeyPurpose variants](./implementation/models.md#public-key-purpose).


### 3. authentication

When the key purpose is ```PublicKeyPurpose.Auth = 'auth'```.

The ```authentication``` value MUST be an array of verification methods. Each verification method MAY be embedded or referenced, e.g.:

```json
{
  "authentication": [
    // referenced key:
    "did:tyron:zil:test:0x5a156a1d18a9a76a0a86b62fcdcb2e547173f3c9#generalKeyID",

    // embedded key:
    {
      "id": "did:tyron:zil:test:0x5a156a1d18a9a76a0a86b62fcdcb2e547173f3c9#onlyAuthKey",
      "type": "SchnorrSecp256k1VerificationKey2019",
      "publicKeyBase58": "jcJRkK8kEu8YFML8bwpkZwzo8YXDFhfFuryfGQAtw4cd"
    }
  ]
}
```

### 4. service

Tyron service endpoints allow communication with the [DID subject](./W3C-dids.md#did-subject), from privacy-preserving messaging services to cryptocurrency addresses.

The ```service``` value MUST be an array of objects of type [DidServiceEndpointModel](./implementation/models.md#did-service-endpoint-model), e.g.:

```json
{
  "service": [
    {
      "id": "did:tyron:zil:test:0x5a156a1d18a9a76a0a86b62fcdcb2e547173f3c9#did-method",
      "type": "website",
      "endpoint": "https://tyronZIL.com"
    },
    {
      "id": "did:tyron:zil:test:0x5a156a1d18a9a76a0a86b62fcdcb2e547173f3c9#account-X",
      "type": "zil-address",
      "endpoint": "zil1egvj6ketfydy48uqzu8qphhj5w4xrkratv85ht"
    }
  ]
}
```

All services MUST have the following properties:

- ***id***: Its value MUST be a unique Tyron DID-URL with a length no more than fifty (50) ASCII encoded characters. There MUST NOT be multiple keys with the same id-value - otherwise the [consumer](./W3C-dids.md#consumer) MUST produce an error
- ***type***: Its value MUST be a string with a length of no more than thirty (30) ASCII encoded characters
- ***endpoint***: Its value MUST be a valid URI string (including a scheme segment, i.e. 'https://', 'git://'), with a length of no more than one hundred (100) ASCII encoded characters OR a Zilliqa address

> If any of the values exceed the specified lengths, the [consumer](./W3C-dids.md#consumer) MUST produce an error.
