# W3C DID-Scheme

A DID-Scheme is the formal syntax of a Decentralized Identifier.

## Tyron DID-Scheme

It is a URI scheme conformant with [RFC3986, Uniform Resource Identifier (URI): Generic Syntax](https://tools.ietf.org/html/rfc3986).

The following is the ABNF definition using the syntax in [RFC5234, "Augmented BNF for Syntax Specifications: ABNF"](https://tools.ietf.org/html/rfc5234):

> Generic rule names not defined there, are defined in [RFC3986](https://tools.ietf.org/html/rfc3986).

```json
did                     = "did:" method-name ":" method-specific-id

method-name             = 1*method-char

method-char             = %x61-7A / DIGIT

method-specific-id      = *( *idchar ":" ) 1*idchar

idchar                  = HEXDIG

HEXDIG                  = DIGIT / "a" / "b" / "c" / "d" / "e" / "f"

DIGIT                   = %x30-39
```

> Both the scheme identifier (did) and the method name MUST be an [ASCII lowercase string](https://infra.spec.whatwg.org/#ascii-lowercase).

The Tyron DID-Method defines its method-name as "tyron" and the method-specific-id syntax as hierarchically partitioned:

```json
method-name             = "tyron"

method-specific-id      = blockchain-namespace ":" network-namespace ":" did-suffix

blockchain-namespace    = "zil:"

network-namespace       = "main:" / "test:"

did-suffix              = 1*idchar
```

**Example of a Tyron DID**:

```did:tyron:zil:test:0x5a156a1d18a9a76a0a86b62fcdcb2e547173f3c9```

### DID-Suffix

The DID-Suffix MUST be globally unique. A Tyron DID-Suffix is the Zilliqa hex-encoded address of the corresponding [DID-Smart-Contract (DSC)](../smart-contracts/DSC.md).

Every Zilliqa address is unique. As explained in [Zilliqa's white-paper](https://docs.zilliqa.com/whitepaper.pdf), the "address for a contract account is computed from the address of its creator and how many transactions the creator account has sent, aka account nonce": 

```
contract_address = LSB160(SHA3-256(address||nonce))
```

where:

- LSB160() returns the rightmost 160 bits of the input,
- SHA3-256() is the SHA-3 hash function that produces 256-bit digests,
- *address* is the address of the creator account, and 
- *nonce* is the creator’s nonce value.

### Implementation

The Tyron DID-Scheme gets implemented by the DidScheme procedure of the DID-SC that generates the Decentralized Identifier and its Tyron Hash.

On testnet:

```
procedure DidScheme()
    this_did =
        let did_prefix = "did:tyron:zil:test:" in
        let did_suffix = builtin to_string _this_address in
        builtin concat did_prefix did_suffix;
    decentralized_identifier := this_did;
    this_th =
        let hash = builtin sha256hash this_did in
        builtin to_bystr hash;
    th = Some{ByStr} this_th;
    tyron_hash := th
end
```

### Normalization

- The DID-Scheme name (did) MUST be lowercase.
- The DID-Method name (tyron) MUST be lowercase.
- Tyron's specific-id MUST follow the rules stated above.

### Persistence

A Tyron DID is bound exclusively and permanently to a single [subject](../W3C-dids.md#did-subject), known as the ***contract_owner***, even after deactivation.
