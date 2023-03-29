# Stream and Buffer

## Table of Contents
- [Stream](#stream)
- [Buffer](#buffer)

## Stream
> ...a stream is a sequence of data elements made available over time. | Wikipedia

> A stream is an abstract interface for working with streaming data in Node.js. The `node:stream` module provides an API for implementing the stream interface. | Node.js

- A sequence of data is being moved from one point to another in chunks.
  - Ex: using the file system module to move text in file A to file B.
  - Ex: media stream.
- Streams allow us to process the data while it is still being sent over.
- A stream may potentially have unlimited data.
- Standard Input/Output/Error are streams that a program can use to communicate with its environment.
### Examples
```js
import { stdin, stdout } from "node:process";

// Readable stream.
stdin.on("data", (data) => {
  // ...
  // Writable stream.
  stdout.write(data);
});
```
```js
import * as readline from "node:readline/promises";
import { stdin, stdout } from "node:process";

// Interface for connecting `stdin` to `stdout`.
const rl = readline.createInterface({ input: stdin, output: stdout });

for await (const input of rl) {
  // ...
}
```

## Buffer
> `Buffer` objects are used to represent a fixed-length sequence of bytes. Many Node.js APIs support `Buffer`s. | Node.js

- Buffer objects allow us to interact with streams of binary data, which can be received through, for example, TCP streams and file system operations.
  - _A buffer object is automatically created by Node.js during a stream._
- **Buffers exist because of the inate nature of streams - data may arrive slower or faster than it is being processed/consumed.**
  - If the data arrives faster than it is being processed, the buffer serves as a place for the "piling" data to wait for their turn to be processed.
  - If the data arrives slower than it is being processed, the buffer keeps the data before relaying to be processed.
- A buffer takes up a small space in memory.
- The term "buffering" means it is gathering/waiting for more data to arrive.
### Examples
```js
import { Buffer } from "node:buffer";

const buff1 = Buffer.alloc(10); // buffer with a fixed length of 10 bytes.

const buff2 = Buffer.from("hello world"); // buffer containing "hello world" in binary.
```

## Reference
[Stream (computing) - Wikipedia](https://en.wikipedia.org/wiki/Stream_(computing))  
[Stream | Node.js v18.15.0 Documentation](https://nodejs.org/dist/latest-v18.x/docs/api/stream.html#stream)  
[Using stdout, stdin, and stderr in Node.js - LogRocket Blog](https://blog.logrocket.com/using-stdout-stdin-stderr-node-js/)  
[Streams API - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API)  
[Buffer | Node.js v18.15.0 Documentation](https://nodejs.org/dist/latest-v18.x/docs/api/buffer.html#buffer)  
[Do you want a better understanding of Buffer in Node.js? Check this out.](https://www.freecodecamp.org/news/do-you-want-a-better-understanding-of-buffer-in-node-js-check-this-out-2e29de2968e8)  
