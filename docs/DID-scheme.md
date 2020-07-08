# tyronZIL DID scheme

A DID scheme is the formal syntax of a decentralized identifier.

The generic W3C DID scheme is a URI scheme conformant with [RFC3986]. The following is the ABNF definition using the syntax in [RFC5234], which defines ALPHA and DIGIT. All other rule names not defined in this ABNF are defined in [RFC3986].

```json
did                = "did:" method-name ":" method-specific-id

method-name        = 1*method-char

method-char        = %x61-7A / DIGIT

method-specific-id = *( ":" *idchar ) 1*idchar

idchar             = ALPHA / DIGIT / "." / "-" / "_"
```

The tyronZIL DID method further restricts the generic W3C DID syntax by defining its method-name, method-namespace and method-specific-id syntax:

```json
method-name = tyron

method-namespace = zil

method-specific-id = to-do
```

## tyronZIL method-specific id

The method-specific-id component of a tyronZIL DID MUST be generated as follows:

to-to

It MUST be globally unique.

## tyronZIL DID method namespace: 'zil'

The tyronZIL method-specific-id format include colons to establish a hierarchically partitioned namespace 'zil', and to identify specific networks of Zilliqa, such as mainnet and testnet.

Therefore, the method-specific-id ABFN rule for tyronZIL is as follows:

Example:

### Normalization

- The DID scheme name (did) MUST be lowercase.
- The DID method name (tyron) MUST be lowercase.
- Case sensitivity and normalization of the value of the method-specific-id rule MUST be defined by the governing DID method specification. to-do

### Persistence

A tyronZIL DID is bound exclusively and permanently to its one and only [subject](./W3C-terminology.md#DID-subject), even after deactivation.

## tyronZIL DID URL syntax

The following is the ABNF definition using the syntax in [RFC5324]. The path-abempty and fragment components are identical to the ABNF rules defined in [RFC3986], and the did-query component is derived from the query ABNF rule."

```json
did-url     = did path-abempty [ "?" did-query ] [ "#" fragment ]

did-query   = param *( "&" param )

param       = param-name "=" param-value

param-name  = 1*pchar

param-value = *pchar
```

## tyronZIL DID URL components

### Path

To be used to address resources available through a service endpoint. It MUST conform to the path-abempty ABFN rule in [RFC3986].

### Query

The W3C did-query component is derived from the query ABFN rule. It MUST be used with DID parameters as follows.

#### DID URL parameters

DID URL parameters are part on the query component of the DID URL to specify what resource is being requested.

W3C DID parameter names:

- **hl**: A resource hash of the DID document to add integrity protection. to-do: see HASHLINK

- **service**: Identifies a service from the DID document by service ID.

    ```did:tyron:zil:91tDAKCERh95uGgKbJNHYp?service=agent```

- **version-id**: Identifies a specific version of the DID document to be resolved.

- **version-time**: Identifies a certain version timestamp of the DID document to be resolved (the document that was valid at that specific time).

    ```did:tyron:zil:91tDAKCERh95uGgKbJNHYp?version-time=2020-09-09T17:00:00Z```

#### Additional method-specific DID URL parameters

A method-specific parameter name MUST be prefixed by the method name, as defined by the method-name rule.

A method-specific parameter name defined by one DID method MAY be used byother DID methods.

to-do: see how this relates to initial-value in sidetree - sugest to use sidetree as the method name.

> Method-specific parameter names MAY be combined with generic parameter names in any order. Method-specific parameter namespaces MAY include colons to be partitioned hierarchically.

### Fragment

A W3C DID fragment is used as a method-independent reference into the DID document to identify a component of the document, e.g. a unique public key description or service endpoint. It MUST conform to the generic URI fragment syntax in [RFC3986]. to-do: read the RFC.

