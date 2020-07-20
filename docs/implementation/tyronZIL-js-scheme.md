# tyronZIL-js DID scheme

**tyronZIL-js** is the open-source reference implementation for Node.js, written in TypeScript.

> To learn the theory behind the tyronZIL DID scheme, go [here](../did-scheme.md)

The tyronZIL-js DID scheme corresponds to the class [TyronZILScheme](https://github.com/julio-cabdu/tyronZIL-js/tree/master/src/lib/tyronZIL-scheme.ts), which has the following properties:

- schemeIdentifier = 'did:'
- methodName = 'tyron:'
- blockchain = 'zil:'
- network = NetworkNamespace
- didUniqueSuffix: string
- did_tyronZIL: string

> did_tyronZIL is the fully expressed tyronZIL DID

The NetworkNamespace is defined as follows:

```js
enum NetworkNamespace {
    Mainnet = 'main:',
    Testnet = 'test:',
}
```
