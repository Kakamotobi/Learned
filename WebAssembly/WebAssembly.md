# WebAssembly

## Table of Contents
- [What is WebAssembly?](#what-is-webassembly)
  - [Some Use Cases](#some-use-cases)

## What is WebAssembly?
> WebAssembly (abbreviated Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications. | WebAssembly

- Wasm defines an AST represented in binary format.
  - Browsers understand binary format. This means that the code will be compiled into a smaller payload compared to JS. Hence, leads to faster delivery.
  - Keypoint is low-level.
- Wasm allows you to run code written in multiple languages on the Web very quickly.
- WebAssembly complements and runs alongside JS.
  - Wasm JS APIs allow you to load wasm modules into JS.
  - Performance critical tasks can be implemented in wasm and imported like a standard JS module.

### Some Use Cases
- Run a whole video game on the browser without having to install anything.
- Run PhotoShop seemlessly on the browser.
- `@ffmpeg/ffmpeg`
  - A C program implemented in wasm.
  - It can be used to transcode audio/video files in the browser.
  - A virtual computer/software with a file system running on the browser.

## Reference
[WebAssembly](https://webassembly.org/)  
[WebAssembly | MDN](https://developer.mozilla.org/en-US/docs/WebAssembly)  
[What is WebAssembly?. Why the future of the web platform… | by Eric Elliott | JavaScript Scene | Medium](https://medium.com/javascript-scene/what-is-webassembly-the-dawn-of-a-new-era-61256ec5a8f6)  
[What is WebAssembly? The next-generation web platform explained](https://www.infoworld.com/article/3291780/what-is-webassembly-the-next-generation-web-platform-explained.html)  
[@ffmpeg/ffmpeg - npm](https://www.npmjs.com/package/@ffmpeg/ffmpeg)  
