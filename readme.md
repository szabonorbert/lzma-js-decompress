# lzma-decompress-js

A standalone JavaScript implementation of the LZMA decompression algorithm.  
Written in pure JS, works everywhere: **Node.js**, **browsers**, **Cloudflare Workers**, and more, with **no dependencies**.

Originally grabbed from [LZMA-JS repository (MIT)](https://github.com/LZMA-JS/LZMA-JS),
I just made some tiny changes to make it work everywhere.

## Installation

Just download the file and include it in your project - no build tools required.

## Dukascopy `.bi5`

I’m 99% sure you came here to decompress **Dukascopy `.bi5`** data. 
Here's how you can do that:

```js
import decompress from "./decompress.js";
...
const res = await fetch(bi5dataURL);
if (!res.ok) return new Response('Failed to load', { status: 500 });
const compressed = new Uint8Array(await res.arrayBuffer());
const decompressed = decompress(compressed);
const dec8 = new Uint8Array(decompressed);
const recordSize = 20;
const view = new DataView(dec8.buffer);
for (let i = 0; i < dec8.length; i += recordSize) {
    const ms = view.getInt32(i, false);
    const ask = view.getInt32(i + 4, false);
    const bid = view.getInt32(i + 8, false);
    const askVol = view.getFloat32(i + 12, false);
    const bidVol = view.getFloat32(i + 16, false);
    console.log(`tick ${i / recordSize}: ms=${ms} ask=${ask} bid=${bid} askVol=${askVol} bidVol=${bidVol}`);
}
```

## License

This project is licensed under the [MIT License](./LICENSE).

## Author

**99%** [Nathan Rugg](https://github.com/nmrugg)  
**1%** small mod and `.bi5` example by [Norbert Szabo](https://github.com/szabonorbert)
