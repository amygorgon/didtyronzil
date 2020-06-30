# tyronZIL DID scheme

A DID scheme is the formal syntax of a decentralized identifier.

The generic DID scheme is a URI scheme conformant with [RFC3986]. The following is the ABNF definition using the syntax in [RFC5234], which defines ALPHA and DIGIT. All other rule names not defined in this ABNF are defined in [RFC3986].

```json
did                = "did:" method-name ":" method-specific-id

method-name        = 1*method-char

method-char        = %x61-7A / DIGIT

method-specific-id = *( ":" *idchar ) 1*idchar

idchar             = ALPHA / DIGIT / "." / "-" / "_"
```

The tyronZIL DID method further restricts the generic DID syntax by defining its own method-name and its own method-specific-id syntax:

```json
method-name = tyron

method-specific-id = to-do
```

### Normalization

- The DID scheme name (did) MUST be lowercase.
- The DID method name (tyron) MUST be lowercase.
- Case sensitivity and normalization of the value of the method-specific-id rule MUST be defined by the governing DID method specification. to-do

### Persistence

A tyronZIL DID is bound exclusively and permanently to its one and only [subject](./W3C-definitions.md#DID-subject), even after deactivation.

## DID URL syntax
