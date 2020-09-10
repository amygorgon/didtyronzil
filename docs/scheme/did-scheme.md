# DID-Scheme

A DID-Scheme is the formal syntax of a Decentralized Identifier.

## Generic W3C DID-Scheme

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

## tyronZIL DID-Scheme

The tyronZIL DID-Method defines its method-name as "tyron" and the method-specific-id syntax as hierarchically partitioned:

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

### DID-Suffix

The DID-Suffix MUST be globally unique and generated as specified in the [tyronZIL DID-create operation](../operations/CRUD/did-create.md).

### Normalization

- The DID-Scheme name (did) MUST be lowercase.
- The DID-Method name (tyron) MUST be lowercase.
- TyronZIL's specific-id MUST follow the rules stated above.

### Persistence

A tyronZIL DID is bound exclusively and permanently to its one and only [subject](../W3C-dids.md#did-subject), even after deactivation.

## Implementation

The tyronZIL-js DID-Scheme corresponds to the class [TyronZILScheme](https://github.com/julio-cabdu/tyronZIL-js/blob/master/src/lib/decentralized-identity/tyronZIL-schemes/did-scheme.ts), which has the following properties:

- schemeIdentifier = 'did:'
- methodName = 'tyron:'
- blockchain = 'zil:'
- network = NetworkNamespace
- didUniqueSuffix: string
- did_tyronZIL: string

> did_tyronZIL is the fully expressed tyronZIL DID
