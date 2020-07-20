# DID scheme

A DID scheme is the formal syntax of a Decentralized Identifier.

## Generic W3C DID scheme

It is a URI scheme conformant with [RFC3986, Uniform Resource Identifier (URI): Generic Syntax](https://tools.ietf.org/html/rfc3986).

The following is the ABNF definition with ALPHA and DIGIT:
> Using the syntax in [RFC5234, Augmented BNF for Syntax Specifications: ABNF](https://tools.ietf.org/html/rfc5234). Generic rule names not defined there, are defined in [RFC3986](https://tools.ietf.org/html/rfc3986)

```json
did                     = "did:" method-name ":" method-specific-id

method-name             = 1*method-char

method-char             = %x61-7A / DIGIT

method-specific-id      = *( *idchar ":" ) 1*idchar

idchar                  = ALPHA / DIGIT / "." / "-" / "_"
```

Both the scheme identifier (did) and the method name MUST be an [ASCII lowercase string](https://infra.spec.whatwg.org/#ascii-lowercase).

## tyronZIL DID scheme

The tyronZIL DID method defines its method-name as "tyron" and the method-specific-id syntax as hierarchically partitioned:

```json
method-name             = "tyron"

method-specific-id      = blockchain-namespace ":" network-namespace ":" did-suffix

blockchain-namespace    = "zil:"

network-namespace       = 1*idchar

did-suffix              = 1*idchar
```

The network-namespace MUST be one of the following variants:

```js
enum NetworkNamespace {
    Mainnet = 'main:',
    Testnet = 'test:',
}
```

**Example of a tyronZIL DID**:

```did:tyron:zil:test:EiAT_GxAt7gBozHlw2B1i7mfQaaORL3NOfFQr9FUt7jp6g```

### DID suffix

The did-suffix MUST be globally unique and generated as specified in the [tyronZIL DID-create operation](./operations/CRUD/did-create.md).

### Normalization

- The DID scheme name (did) MUST be lowercase.
- The DID method name (tyron) MUST be lowercase.
- TyronZIL's specific-id MUST follow the rules stated above.

### Persistence

A tyronZIL DID is bound exclusively and permanently to its one and only [subject](./W3C-dids.md#did-subject), even after deactivation.

## tyronZIL DID URL syntax

The following is the ABNF definition using the syntax in [RFC5324](https://tools.ietf.org/html/rfc5234):

> The path-abempty and fragment components are identical to the ABNF rules defined in [RFC3986](https://tools.ietf.org/html/rfc3986)  
> The did-query component is derived from the query ABNF rule

```json
did-url     = did path-abempty [ "?" did-query ] [ "#" fragment ]

did-query   = param *( "&" param )

param       = param-name "=" param-value

param-name  = 1*pchar

param-value = *pchar
```

### Path

To be used to address resources available through a service endpoint. It MUST conform to the path-abempty ABFN rule in [RFC3986](https://tools.ietf.org/html/rfc3986).

### Query

The W3C did-query component derives from the query ABFN rule. It MUST be used with DID parameters as follows.

#### DID URL parameters

These parameters are part of the query component of the DID URL to specify what resource is requested.

W3C DID parameter names:

- **hl**: A resource hash of the DID document to add integrity protection.

- **service**: Identifies a service from the DID document by service ID.

    ```did:tyron:zil:test:EiAT_GxAt7gBozHlw2B1i7mfQaaORL3NOfFQr9FUt7jp6g?service=agentId1```

- **version-id**: Identifies a specific version of the DID document to be resolved.

- **version-time**: Identifies a specific version timestamp of the DID document to be resolved (the doc that was valid at that particular time).

    ```did:tyron:zil:test:EiAT_GxAt7gBozHlw2B1i7mfQaaORL3NOfFQr9FUt7jp6g?version-time=2020-09-07T17:00:00Z```

Additional parameters MUST be prefixed by the method name 'tyron', e.g.: 'tyron-dns'. TyronZIL method-specific parameter names MAY be used by other DID methods.

> Method-specific parameter names MAY be combined with generic parameter names in any order. Method-specific parameter namespaces MAY include colons to be partitioned hierarchically.

### Fragment

A W3C DID fragment is used as a method-independent reference into the DID document to identify a component of the document by ID, e.g. a specific public key or service endpoint. It MUST conform to the generic URI fragment syntax in [RFC3986](https://tools.ietf.org/html/rfc3986).

## Implementation

Go to: [tyronZIL-js DID scheme implementation](./implementation/tyronZIL-js-scheme.md)