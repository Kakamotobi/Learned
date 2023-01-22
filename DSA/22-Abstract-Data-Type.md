# Abstract Data Type

## Table of Contents
- [What is an Abstract Data Type(ADT)?](#what-is-an-abstract-data-typeadt)
  - [Some ADTs](#some-adts)
- [List](#list)
- [Array](#array)
- [JS Array](#js-array)
  - [JS Typed Arrays](#js-typed-arrays)
    - [Typed Array Architecture](#typed-array-architecture)
      - [Buffer](#buffer)
        - [ArrayBuffer](#arraybuffer)
      - [View](#view)
      - [Examples](#examples)
  - [How V8 Manages Array Memory Location](#how-v8-manages-array-memory-location)
    - [General Rule](#general-rule)
    - [Test Cases](#test-cases)

## What is an Abstract Data Type(ADT)?
> ...an abstract data type (ADT) is a mathematical model for data types. An abstract data type is defined by its behavior (semantics) from the point of view of a user, of the data, specifically in terms of possible values, possible operations on data of this type, and the behavior of these operations. This mathematical model contrasts with data structures, which are concrete representations of data, and are the point of view of an implementer, not a user. | Wikipedia

> Abstract data types are purely theoretical entities, used (among other things) to simplify the description of abstract algorithms, to classify and evaluate data structures, and to formally describe the type systems of programming languages. However, an ADT may be implemented by specific data types or data structures, in many ways and in many programming languages; or described in a formal specification language. | Wikipedia

- Data types are defined abstractly, and the internals of the "how" can be implemented in different ways. 
- **However, each implementation of the same data type must provide the same interface**.
- For example, in general, an Array requires to provide a time complexity of O(1) for random access using indices. This does not enforce that Arrays must be stored contiguously in memory. Therefore, different languages and/or engines are free to implement this O(1) random access using indices in any way. Some may decide to store the items contiguously in a fixed array internally, and others may decide to use a dictionary internally instead. It doesn't matter as long as the interface is the same.
### Some ADTs
- List
- Stack
- Queue

## List
> ...a **list** or **sequence** is an abstract data type that represents a finite number of ordered values, where the same value may occur more than once. | Wikipedia

- A list is simply a sequential ADT that supports adding/inserting, reassigning, deleting, searching of items.
- Lists are usually implemented as linked lists or arrays.
- *Note*
  - The word "list" is used to refer to concrete data structures (Ex: linked lists, arrays) that are used to implement other common abstract data types.

## Array
> ...an **array** is a data structure consisting of a collection of elements (values or variables), each identified by at least one array index or key. | Wikipedia

- An array is simply a data structure that requires random access to be O(1) through the use of indices.
  - *It is NOT the definition of an array to being stored contiguously in memory.*
  - Therefore, an array can be implemented to be contiguous or not contiguous in memory as long as an array item can be accessed through its index.

## JS Array
- JS arrays are resizable and can contain different data types.
### JS Typed Arrays
> **JavaScript typed arrays** are array-like objects that provide a mechanism for reading and writing raw binary data in memory buffers. | MDN

- **In a JS typed array, each entry is raw binary value represented in a supported format (8-bit integers, 64-bit floating-point numbers, etc.).**
- A typed array has some common array methods. However, it is *not an ordinary array*.
  - `Array.isArray(typedArr); // false`
- In cases such as manipulating audio/video, or dealing with WebSockets, we need to quickly and easily manipulate raw binary data in JS code.
- Typed arrays are iterable. Therefore, they can be converted to ordinary arrays using the spread syntax or `Array.from()`.
#### Typed Array Architecture
- A typed array is implemented using two things: **buffers** and **views**.
##### Buffer
- A **buffer** is an object that represents a portion of memory that is set aside as a temporary holding place for data.
- A buffer has no format or mechanism to access its contents.
###### ArrayBuffer
> The **`ArrayBuffer`** object is used to represent a generic, fixed-length raw binary data buffer. | MDN

> It is an array of bytes, often referred to in other languages as a "byte array". You cannot directly manipulate the contents of an ArrayBuffer; instead, you create one of the typed array objects or a DataView object which represents the buffer in a specific format, and use that to read and write the contents of the buffer. | MDN
##### View
> A view provides a *context* - that is, a data type, starting offset, and number of elements - that turns the data into an actual typed array. | MDN

- A view is needed to access the memory contained in a buffer.
- Typed array views [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays#typed_array_views).
##### Examples
###### Example 1
- Creating an 8 byte buffer with an Int32Array view, with contents initialized to 0.
```js
const buffer = new ArrayBuffer(8);
buffer.byteLength; // 8

const view = new Int32Array(buffer); // Int32Array(2) [ 0, 0 ]
```
```
> %DebugPrint(buffer)

DebugPrint: 0x8f311fc5451: [JSArrayBuffer] in OldSpace
 ...
 - elements: 0x08f35e5c1329 <FixedArray[0]> [HOLEY_ELEMENTS]
 - embedder fields: 2
 - backing_store: 0x600000bec0f0
 - byte_length: 8
 - max_byte_length: 8
 - detachable
 - properties: 0x08f35e5c1329 <FixedArray[0]>
 - All own properties (excluding elements): {}
 - embedder fields = {
    0, aligned pointer: 0x0
    0, aligned pointer: 0x0
 }
```
```
> %DebugPrint(view)

DebugPrint: 0x21cb93d49469: [JSTypedArray]
 ...
 - elements: 0x21cb56d83269 <ByteArray[0]> [INT32ELEMENTS]
 - embedder fields: 2
 - buffer: 0x21cbba634149 <ArrayBuffer map = 0x21cbaed42601>
 - byte_offset: 0
 - byte_length: 8
 - length: 2
 ...
 - elements: 0x21cb56d83269 <ByteArray[0]> {
         0-1: 0
 }
```
###### Example 2
- Sharing the same data/buffer (8 byte array) with multiple views (16-bit integer view and 32-bit integer view).
- The two arrays view the same data buffer but treat them in different formats.
```js
const buffer = new ArrayBuffer(8);

const view1 = new Int16Array(buffer); // Int16Array(4) [ 0, 0, 0, 0 ]
const view2 = new Int32Array(buffer); // Int32Array(2) [ 0, 0 ]

view1[0] = 3;
buffer; // <03 00 00 00 00 00 00 00> <-- hex
view1; // [ 3, 0, 0, 0 ] <-- Int16 Little-Endian (03 00 / 00 00 / 00 00 / 00 00)
view2; // [ 3, 0 ] <-- Int32 Little-Endian (03 00 00 00 / 00 00 00 00)

view1[1] = 5;
buffer; // <03 00 05 00 00 00 00 00> <-- hex
view1; // [ 3, 5, 0, 0 ] <-- Int16 Little-Endian (03 00 / 05 00 / 00 00 / 00 00)
view2; // [ 327683, 0 ] <-- Int32 Little-Endian (03 00 05 00 / 00 00 00 00)
```

### How V8 Manages Array Memory Location
- When more elements are added to the array beyond its internal size (buffer), V8 copies over the array to another memory address (reallocation) in order to accommodate more buffer.
  - `new_capacity = (old_capacity + 50%) + 16`
- `<the_hole>`
  - `<the_hole>` indicates unassigned or deleted array items in the internal array.
  - Buffers are also indicated by `<the_hole>` (since they are unassigned space). Therefore, packed arrays can also contain `<the_hole>` (it doesn't mean they are holey).
- `new Array(n) vs []`
  - new Array(n)
    - (+) Preallocate space in memory for n elements (i.e. FixedArray).
    - (+) Optimizes array creation.
    - (-) Creates a holey array.
    - (-) Slower array operations (compared to packed arrays).
  - arr = []; arr.push(x)
    - (+) Creates a packed array.
    - (+) Optimizes array operations.
    - (-) Need to allocate space as the array grows (buffer).
    - (-) Slower array creation.
- Some Tips
  - Avoid creating holes.
  - Avoid out-of-bounds reads.
    - Ex: avoid stopping a loop when an `undefined` is encountered.
- `node --allow-natives-syntax` to use `%DebugPrint()`.
#### General Rule
- *This is not an accurate rule and V8's internal optimization is more complex than this.*
- Dependent Variables: array length/size, holey-ness.
```js
if (array is small) {
  use FixedArray; // regardless of packed or holey
} else if (array is large) {
  if (packed || few holes) use FixedArray;
  else if (lots of holes) use Dictionary;
}
```
- Observation - when Small Array becomes Large Array
  ```js
  const arr = [];
  arr[1023] = 1023; // <FixedArray[1552]> [HOLEY_SMI_ELEMENTS]
  ```
  ```js
  const arr = [];
  arr[1024] = 1024; // <NumberDictionary[16]> [DICTIONARY_ELEMENTS]
  ```
#### Test Cases
##### Small and Packed Array
- **Internal Data Structure:** FixedArray
- **Elements Type:** PACKED_SMI_ELEMENTS
```js
const arr = [];
for (let i = 0; i <= 10; i++) arr.push(i);
// [ 0, 1, 2, ..., 9, 10 ]
```
```
DebugPrint: 0xce554502ec9: [JSArray] in OldSpace
 ...
 - elements: 0x0ce56a1030a1 <FixedArray[17]> [PACKED_SMI_ELEMENTS]
 - length: 11
 ...
 - elements: 0x0ce56a1030a1 <FixedArray[17]> {
           0: 0
           1: 1
           2: 2
           ...
           9: 9
          10: 10
       11-16: 0x0ce52d081689 <the_hole>
 }
```
##### Small and Holey Array
- **Internal Data Structure:** FixedArray
- **Elements Type:** HOLEY_SMI_ELEMENTS
```js
const arr = [];
arr[0] = 0;
arr[10] = 10;
// [ 0, <9 empty items>, 10 ]
```
```
DebugPrint: 0x1ab7106cda61: [JSArray] in OldSpace
 ...
 - elements: 0x1ab7106d5f51 <FixedArray[17]> [HOLEY_SMI_ELEMENTS]
 - length: 11
 ...
 - elements: 0x1ab7106d5f51 <FixedArray[17]> {
           0: 0
         1-9: 0x1ab765bc1689 <the_hole>
          10: 10
       11-16: 0x1ab765bc1689 <the_hole>
 }
```
##### Large and Holey Array
###### Large and Holey Array with few Holes
- **Internal Data Structure:** FixedArray
- **Elements Type:** HOLEY_SMI_ELEMENTS
```js
const arr = [];
arr[0] = 0;
arr[10] = 10;
for (let i = 11; i <= 1000000; i++) arr.push(i);
// [ 0, <9 empty items>, 10, 11, 12, ... 999999, 1000000 ]
```
```
DebugPrint: 0x2b247694c491: [JSArray] in OldSpace
 ...
 - elements: 0x2b24ad6c1131 <FixedArray[1304209]> [HOLEY_SMI_ELEMENTS]
 - length: 1000001
 ...
 - elements: 0x2b24ad6c1131 <FixedArray[1304209]> {
              0: 0
            1-9: 0x2b248f641689 <the_hole>
             10: 10
             11: 11
             12: 12
              ...
         999999: 999999
        1000000: 1000000
1000001-1304208: 0x2b248f641689 <the_hole>
 }
```
###### Large and Holey Array with lots of Holes
- **Internal Data Structure:** Dictionary
- **Elements Type:** DICTIONARY_ELEMENTS
```js
const arr = [];
arr[0] = 0;
arr[1000000] = 1000000;
// [ 0, <999999 empty items>, 1000000 ]
```
```
DebugPrint: 0xec10d7492a9: [JSArray] in OldSpace
 ...
 - elements: 0x0ec1fc7029e1 <NumberDictionary[16]> [DICTIONARY_ELEMENTS]
 - length: 1000001
 ...
 - elements: 0x0ec1fc7029e1 <NumberDictionary[16]> {
   - max_number_key: 1000000
   0: 0 (data, dict_index: 0, attrs: [WEC])
   1000000: 1000000 (data, dict_index: 0, attrs: [WEC])
 }
```
##### Large and Packed Array
- **Internal Data Structure:** FixedArray
- **Elements Type:** PACKED_SMI_ELEMENTS
```js
const arr = [];
for (let i = 0; i <= 1000000; i++) arr.push(i);
// [ 0, 1, 2, ... 999999, 1000000 ]
```
```
DebugPrint: 0xd800bab2001: [JSArray] in OldSpace
 ...
 - elements: 0x0d80a5f81131 <FixedArray[1304209]> [PACKED_SMI_ELEMENTS]
 - length: 1000001
 ...
 - elements: 0x0d80a5f81131 <FixedArray[1304209]> {
           0: 0
           1: 1
           2: 2
            ...
      999999: 999999
     1000000: 1000000
1000001-1304208: 0x0d801a041689 <the_hole>
 }
```

## Reference
[Abstract data type - Wikipedia](https://en.wikipedia.org/wiki/Abstract_data_type)  
[List (abstract data type) - Wikipedia](https://en.wikipedia.org/wiki/List_(abstract_data_type))  
[Array (data structure) - Wikipedia](https://en.wikipedia.org/wiki/Array_(data_structure))  
[JavaScript typed arrays - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays)  
[ArrayBuffer - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)  
[Online Hex Converter - Bytes, Ints, Floats, Significance, Endians - SCADACore](https://www.scadacore.com/tools/programming-calculators/online-hex-converter/)  
