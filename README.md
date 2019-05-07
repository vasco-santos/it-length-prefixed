# it-length-prefixed

[![](https://img.shields.io/badge/made%20by-Protocol%20Labs-blue.svg?style=flat-square)](http://ipn.io)
[![](https://img.shields.io/badge/project-IPFS-blue.svg?style=flat-square)](http://ipfs.io/)
[![](https://img.shields.io/badge/freenode-%23ipfs-blue.svg?style=flat-square)](http://webchat.freenode.net/?channels=%23ipfs)
[![Travis CI](https://travis-ci.org/alanshaw/it-length-prefixed.svg?branch=master&style=flat-square)](https://travis-ci.org/alanshaw/it-length-prefixed)
![Codecov](https://img.shields.io/codecov/c/gh/alanshaw/it-length-prefixed.svg?style=flat-square)
[![Dependency Status](https://david-dm.org/alanshaw/it-length-prefixed.svg?style=flat-square)](https://david-dm.org/alanshaw/it-length-prefixed)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/feross/standard)
[![standard-readme compliant](https://img.shields.io/badge/standard--readme-OK-green.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)

> Streaming length prefixed buffers with async iterators

## Install

```sh
npm install it-length-prefixed
```

## Usage

```js
const pipe = require('it-pipe')
const lp = require('it-length-prefixed')

const encoded = []

// encode
await pipe(
  [Buffer.from('hello world')],
  lp.encode(),
  async source => {
    for await (const chunk of source) {
      encoded.push(chunk)
    }
  }
)

console.log(encoded)
// => [Buffer <0b 68 65 6c 6c 6f 20 77 6f 72 6c 64>]

const decoded = []

// decode
await pipe(
  encoded, // e.g. from above
  lp.decode(),
  async source => {
    for await (const chunk of source) {
      decoded.push(chunk)
    }
  }
)

console.log(decoded)
// => [Buffer <68 65 6c 6c 6f 20 77 6f 72 6c 64>]
```

## API

### `encode([opts])`

- `opts: Object`, optional
  - `poolSize: 10 * 1024`: Buffer pool size to allocate up front

All messages will be prefixed with a varint.

Returns a [transform](https://gist.github.com/alanshaw/591dc7dd54e4f99338a347ef568d6ee9#transform-it).

### `decode([opts])`

- `opts: Object`, optional
  - `maxDataLength`: If provided, will not decode messages longer than the size specified, if omitted will use the current default of 4MB.

All messages will be prefixed with a varint.

Returns a [transform](https://gist.github.com/alanshaw/591dc7dd54e4f99338a347ef568d6ee9#transform-it).

## Contribute

PRs and issues gladly accepted! Check out the [issues](https://github.com/alanshaw/it-length-prefixed/issues).

## License

MIT © 2016 Friedel Ziegelmayer
