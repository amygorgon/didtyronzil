# tyronZIL DID-URL syntax

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

### DID-URL parameters

These parameters are part of the query component of the DID-URL to specify what resource is requested.

W3C DID parameter-names:

- **hl**: A resource hash of the DID-Document to add integrity protection.

- **service**: Identifies a service from the DID-Document by service ID.

    ```did:tyron:zil:test:EiAT_GxAt7gBozHlw2B1i7mfQaaORL3NOfFQr9FUt7jp6g?service=agentId1```

- **version-id**: Identifies a specific version of the DID-Document to be resolved.

- **version-time**: Identifies a specific version timestamp of the DID-Document to be resolved (the doc that was valid at that particular time).

    ```did:tyron:zil:test:EiAT_GxAt7gBozHlw2B1i7mfQaaORL3NOfFQr9FUt7jp6g?version-time=2020-09-07T17:00:00Z```

Additional parameters MUST be prefixed by the method name 'tyron', e.g.: 'tyron-dns'. TyronZIL method-specific parameter names MAY be used by other DID methods.

> Method-specific parameter names MAY be combined with generic parameter names in any order. Method-specific parameter namespaces MAY include colons to be partitioned hierarchically.

At the current version, the tyronZIL-Method does not support DID-URL parameters, EXCEPT the following:

#### Sidetree Long-Form DID

By using a DID with the DID-URL parameter ```sidetree-initial-state```, a tyronZIL user can utilize their [Long-Form DID URI](../sidetree.md#long-form-did-uri). It is composed by the [Create Operation Suffix Data Object](../operations/CRUD/did-create.md#create-operation-suffix-data-object) and the [Create Operation Delta Object](../operations/CRUD/did-create.md#create-operation-delta-object), separated by a period '```.```':

```did:tyron:zil:test:EiApcQfeTVd3aCGb07Cj3MfZBaBv6KA7kdCAokuM6qxNWQ?sidetree-initial-state=eyJkZWx0YV9oYXNoIjoiRWlCWS1yRm1NVDNESlV0ZktaYnNqSFJyVXRjbXVCUHZ2M2htemhZb3pia3g0dyIsInJlY292ZXJ5X2NvbW1pdG1lbnQiOiJFaUF0LVJpV29uU1Nsc1U5SWVsbUY3MHZsZ1oybVpyU29QT0Y2RWJybmMya1dBIn0.eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJyZXBsYWNlIiwiZG9jdW1lbnQiOnsicHVibGljX2tleXMiOlt7ImlkIjoicHJpbWFyeVNpZ25pbmdLZXkiLCJ0eXBlIjoiRWNkc2FTZWNwMjU2azFWZXJpZmljYXRpb25LZXkyMDE5IiwiandrIjp7Imt0eSI6IkVDIiwiY3J2Ijoic2VjcDI1NmsxIiwieCI6IjFsQlBRaGtoNS02U3plNUlkbWZFeUZWVXdXUWFsYTVjcE5QWkJ6bU4zd1kiLCJ5IjoidEdYTkZIWDhjMkVsdkdyS2xPeHdxOXNHUDFVVFh2aW1SZmJUQy1yd3F5RSIsImtpZCI6IkplOEVOSk4ydm1oNVAwS0FXeEVwbTJQSllaMjBPZzNNSlFGSm1kdVE2T0EifSwicHVycG9zZSI6WyJnZW5lcmFsIiwiYXV0aCJdfV0sInNlcnZpY2VfZW5kcG9pbnRzIjpbeyJpZCI6InR5cm9uWklMLXdlYnNpdGUiLCJ0eXBlIjoibWV0aG9kLXNwZWNpZmljYXRpb24iLCJlbmRwb2ludCI6Imh0dHBzOi8vdHlyb25aSUwuY29tIn0seyJpZCI6IlpJTC1hZGRyZXNzIiwidHlwZSI6ImNyeXB0b2N1cnJlbmN5LWFkZHJlc3MiLCJlbmRwb2ludCI6InppbDFlZ3ZqNmtldGZ5ZHk0OHVxenU4cXBoaGo1dzR4cmtyYXR2ODVodCJ9XX19XSwidXBkYXRlQ29tbWl0bWVudCI6IkVpQjFnZkZtUVY4b0w3ZFJ5S3hrLU9xeEJ5amJwNTZkd1otNm9sRkpfRWRlWkEifQ```

Find the tyronZIL DID-URL implementation [here](https://github.com/julio-cabdu/tyronZIL-js/blob/master/src/lib/decentralized-identity/tyronZIL-schemes/did-url-scheme.ts)

## Fragment

A W3C DID fragment is used as a method-independent reference into the DID-Document to identify a component of the document by ID, e.g. a specific public key or service endpoint. It MUST conform to the generic URI fragment syntax in [RFC3986](https://tools.ietf.org/html/rfc3986).
