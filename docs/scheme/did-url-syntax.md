# tyronZIL DID URL syntax

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

## Path

To be used to address resources available through a service endpoint. It MUST conform to the path-abempty ABFN rule in [RFC3986](https://tools.ietf.org/html/rfc3986).

## Query

The W3C did-query component derives from the query ABFN rule. It MUST be used with DID parameters as follows.

### DID URL parameters

These parameters are part of the query component of the DID URL to specify what resource is requested.

W3C DID parameter-names:

- **hl**: A resource hash of the DID-document to add integrity protection.

- **service**: Identifies a service from the DID-document by service ID.

    ```did:tyron:zil:test:EiAT_GxAt7gBozHlw2B1i7mfQaaORL3NOfFQr9FUt7jp6g?service=agentId1```

- **version-id**: Identifies a specific version of the DID-document to be resolved.

- **version-time**: Identifies a specific version timestamp of the DID-document to be resolved (the doc that was valid at that particular time).

    ```did:tyron:zil:test:EiAT_GxAt7gBozHlw2B1i7mfQaaORL3NOfFQr9FUt7jp6g?version-time=2020-09-07T17:00:00Z```

Additional parameters MUST be prefixed by the method name 'tyron', e.g.: 'tyron-dns'. TyronZIL method-specific parameter names MAY be used by other DID methods.

> Method-specific parameter names MAY be combined with generic parameter names in any order. Method-specific parameter namespaces MAY include colons to be partitioned hierarchically.

## Fragment

A W3C DID fragment is used as a method-independent reference into the DID-document to identify a component of the document by ID, e.g. a specific public key or service endpoint. It MUST conform to the generic URI fragment syntax in [RFC3986](https://tools.ietf.org/html/rfc3986).
