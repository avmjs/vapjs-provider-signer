# User Guide

All information for developers using `vapjs-provider-signer` should consult this document.

## Install

```
npm install --save vapjs-provider-signer
```

## Usage

```js
const SignerProvider = require('vapjs-provider-signer');
const sign = require('vapjs-signer').sign;
const Vap = require('vapjs-query');
const provider = new SignerProvider('https://ropsten.infura.io', {
  signTransaction: (rawTx, cb) => cb(null, sign(rawTx, '0x...privateKey...')),
});
const vap = new Vap(provider);

vap.sendTransaction({
  from: '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
  gas: 300000,
  data: '0x...',
}, cb);

// results null 0x... (transaction hash)
```

## API Design

### constructor

[index.js:vapjs-provider-signer](../../../blob/master/src/index.js "Source code on GitHub")

Intakes a `provider` URL specified as a string, and an options object where the `privateKey` method is specified.

**Parameters**

-   `provider` **String** the URL path to your local Http RPC enabled Vapory node (e.g. `http://localhost:8545`) or a service node system like [Infura.io](http://infura.io) (e.g. `http://ropsten.infura.io`).
-   `options` **Object** the options object where the `signTransaction` method and `timeout` property is specified.

Example options **Object**:

```js
const options = {
  signTransaction: (rawTx, cb) => {
    if (rawTx.from === '0x...') {
      cb(null, sign(rawTx, '0x...privateKey...'));
    } else {
      cb('some error');
    }
  },
  timeout: 400,
};
```

Result `SignerProvider` **Object**.

```js
const SignerProvider = require('vapjs-provider-signer');
const Vap = require('vapjs-query');
const vap = new Vap(new SignerProvider('http://ropsten.infura.io', {
  signTransaction: (rawTx, cb) => cb(null, SignerProvider.sign(rawTx, '0x...privateKey...')),
}));

vap.sendTransaction({
  from: '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
  gas: 300000,
  data: '0x...',
}, cb);

// results null 0x... (transaction hash)
```

## Browser Builds

`vapjs` provides production distributions for all of its modules that are ready for use in the browser right away. Simply include either `dist/vapjs-provider-signer.js` or `dist/vapjs-provider-signer.min.js` directly into an HTML file to start using this module. Note, an `SignerProvider` object is made available globally.

```html
<script type="text/javascript" src="vapjs-provider-signer.min.js"></script>
<script type="text/javascript">
new SignerProvider(...);
</script>
```

Note, even though `vapjs` should have transformed and polyfilled most of the requirements to run this module across most modern browsers. You may want to look at an additional polyfill for extra support.

Use a polyfill service such as `Polyfill.io` to ensure complete cross-browser support:
https://polyfill.io/

## Webpack Figures

```
Hash: f8ea69e4b882307e0d88                                                         
Version: webpack 2.1.0-beta.15
Time: 144ms
                       Asset     Size  Chunks             Chunk Names
    vapjs-provider-signer.js  7.99 kB       0  [emitted]  main
vapjs-provider-signer.js.map  9.27 kB       0  [emitted]  main
   [0] ./lib/index.js 2.45 kB {0} [built]
   [3] multi main 28 bytes {0} [built]
    + 2 hidden modules

                       Asset     Size  Chunks             Chunk Names
vapjs-provider-signer.min.js  3.42 kB       0  [emitted]  main
   [0] ./lib/index.js 2.45 kB {0} [built]
       [3] 1ms -> factory:16ms building:38ms = 55ms
   [3] multi main 28 bytes {0} [built]
       factory:0ms building:1ms = 1ms
    + 2 hidden modules
```

## Other Awesome Modules, Tools and Frameworks

 - [web3.js](https://github.com/vaporyco/web3.js) -- the original Vapory swiss army knife **Vapory Foundation**
 - [vaporyjs](https://github.com/vaporycojs) -- critical vaporyjs infrastructure **Vapory Foundation**
 - [browser-solidity](https://vapory.github.io/browser-solidity) -- an in browser Solidity IDE **Vapory Foundation**
 - [wafr](https://github.com/silentcicero/wafr) -- a super simple Solidity testing framework
 - [truffle](https://github.com/ConsenSys/truffle) -- a solidity/js dApp framework
 - [embark](https://github.com/iurimatias/embark-framework) -- a solidity/js dApp framework
 - [dapple](https://github.com/nexusdev/dapple) -- a solidity dApp framework
 - [chaitherium](https://github.com/SafeMarket/chaithereum) -- a JS web3 unit testing framework
 - [contest](https://github.com/DigixGlobal/contest) -- a JS testing framework for contracts

## Our Relationship with Vapory & VaporyJS

 We would like to mention that we are not in any way affiliated with the Vapory Foundation or `vaporyjs`. However, we love the work they do and work with them often to make Vapory great! Our aim is to support the Vapory ecosystem with a policy of diversity, modularity, simplicity, transparency, clarity, optimization and extensibility.

 Many of our modules use code from `web3.js` and the `vaporyjs-` repositories. We thank the authors where we can in the relevant repositories. We use their code carefully, and make sure all test coverage is ported over and where possible, expanded on.
