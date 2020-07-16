# DID scheme

A DID scheme is the formal syntax of a decentralized identifier.

## Generic W3C DID scheme

URI scheme conformant with [RFC3986].

ABNF definition with ALPHA and DIGIT:
> Using the syntax in [RFC5234]  
> Generic rule names not defined here are defined in [RFC3986]

```json
did                     = "did:" method-name ":" method-specific-id

method-name             = 1*method-char

method-char             = %x61-7A / DIGIT

method-specific-id      = *( *idchar ":" ) 1*idchar

idchar                  = ALPHA / DIGIT / "." / "-" / "_"
```

## tyronZIL DID scheme

The tyronZIL DID method defines its method-name as "tyron" and the method-specific-id syntax as hierarchically partitioned:

```json
method-name             = "tyron"

method-specific-id      = blockchain-namespace ":" network-namespace ":" did-suffix

blockchain-namespace    = "zil:"

network-namespace       = 1*idchar

did-suffix              = 1*idchar
```

The network-namespace can be, e.g. "main" or "test".

### DID suffix

The did-suffix MUST be globally unique and generated as follows, in compliance with the Sidetree protocol:

1. Create key pairs using the [operationKeyPair](./sidetree/sidetree.md#operation-key-pair):

    - The public keys are of type [PublicKeyModel](./sidetree/models.md#public-key-model)
    - The private keys have type JwkEs256k - to-do clarify type

2. Create service endpoints of type [ServiceEndpointModel](./sidetree/models.md#service-endpoint-model)

Create a new DOCUMENT of type DocumentModel:

```json
{
    publicKeys: SIGNING_KEY
}
2.
3.
4.

### Normalization

- The DID scheme name (did) MUST be lowercase.
- The DID method name (tyron) MUST be lowercase.
- TyronZIL's specific-id MUST follow the rules stated above.

### Persistence

A tyronZIL DID is bound exclusively and permanently to its one and only [subject](./W3C-dids.md#did-subject), even after deactivation.

## tyronZIL DID URL syntax

ABNF definition using the syntax in [RFC5324]:

> The path-abempty and fragment components are identical to the ABNF rules defined in [RFC3986]  
> The did-query component is derived from the query ABNF rule

```json
did-url     = did path-abempty [ "?" did-query ] [ "#" fragment ]

did-query   = param *( "&" param )

param       = param-name "=" param-value

param-name  = 1*pchar

param-value = *pchar
```

#### Path

To be used to address resources available through a service endpoint. It MUST conform to the path-abempty ABFN rule in [RFC3986].

#### Query

The W3C did-query component is derived from the query ABFN rule. It MUST be used with DID parameters as follows.

**DID URL parameters** are part on the query component of the DID URL to specify what resource is being requested.

W3C DID parameter names:

- **hl**: A resource hash of the DID document to add integrity protection. to-do: see HASHLINK

- **service**: Identifies a service from the DID document by service ID.

    ```did:tyron:zil:91tDAKCERh95uGgKbJNHYp?service=agent```

- **version-id**: Identifies a specific version of the DID document to be resolved.

- **version-time**: Identifies a certain version timestamp of the DID document to be resolved (the document that was valid at that specific time).

    ```did:tyron:zil:91tDAKCERh95uGgKbJNHYp?version-time=2020-09-09T17:00:00Z```

**Additional parameters** MUST be prefixed by the method name 'tyron', e.g.:

- **tyron-dns**

tyronZIL method-specific parameter names MAY be used by other DID methods.

to-do: see how this relates to initial-value in sidetree - sugest to use sidetree as the method name.

> Method-specific parameter names MAY be combined with generic parameter names in any order. Method-specific parameter namespaces MAY include colons to be partitioned hierarchically.

#### Fragment

A W3C DID fragment is used as a method-independent reference into the DID document to identify a component of the document by ID, e.g. a specific public key or service endpoint. It MUST conform to the generic URI fragment syntax in [RFC3986].
to-do: read the RFC.
