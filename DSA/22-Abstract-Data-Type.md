# Abstract Data Type

## Table of Contents
- [What is Abstract Data Type(ADT)?](#what-is-abstract-data-typeadt)
- [JS Array ft. V8](#js-array-ft-v8)
  - [JS Typed Arrays](#js-typed-arrays)
  - [ArrayBuffer](#arraybuffer)
  - [How V8 Manages Array Memory Location](#how-v8-manages-array-memory-location)
    - [General Rule](#general-rule)
    - [Testcases](#testcases)

## What is Abstract Data Type(ADT)?
> ...an abstract data type (ADT) is a mathematical model for data types. An abstract data type is defined by its behavior (semantics) from the point of view of a user, of the data, specifically in terms of possible values, possible operations on data of this type, and the behavior of these operations. This mathematical model contrasts with data structures, which are concrete representations of data, and are the point of view of an implementer, not a user. | Wikipedia

> Abstract data types are purely theoretical entities, used (among other things) to simplify the description of abstract algorithms, to classify and evaluate data structures, and to formally describe the type systems of programming languages. However, an ADT may be implemented by specific data types or data structures, in many ways and in many programming languages; or described in a formal specification language. | Wikipedia

- Data types are defined abstractly, and the internals of the "how" can be implemented in different ways. 
- **However, each implementation of the same data type must provide the same interface**.
- For example, in general, an Array requires to provide a time complexity of O(1) for random access using indices. This does not enforce that Arrays must be stored contiguously in memory. Therefore, different languages and/or engines are free to implement this O(1) random access using indices in any way. Some may decide to store the items contiguously in a fixed array internally, and others may decide to use a dictionary internally instead. It doesn't matter as long as the interface is the same.

## JS Array
### JS Typed Arrays
- blahblah

### ArrayBuffer
> The **`ArrayBuffer`** object is used to represent a generic, fixed-length raw binary data buffer. | MDN

> It is an array of bytes, often referred to in other languages as a "byte array". You cannot directly manipulate the contents of an ArrayBuffer; instead, you create one of the typed array objects or a DataView object which represents the buffer in a specific format, and use that to read and write the contents of the buffer. | MDN

- A **buffer** is a portion of memory that is set aside as a temporary holding place for data.
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
[JavaScript typed arrays - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays)  
[ArrayBuffer - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)  
