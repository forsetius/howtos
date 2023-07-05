# Buffer

The **buffer** is a chunk of memory and it is similar to an array of numbers.
- the size of a buffer is set when it is created and can’t be changed afterwards
- buffer is an **array of bytes**

## Byte values in a `Buffer`

Since a maximum number saved on a single byte is 255, the buffer element can’t contain bigger numbers. If you try to assign a value bigger than 255, it gets divided by 256 and the remainder of the division is assigned to the element.

```js
const buffer = new Buffer(5);

buffer[0] = 255;
console.log(buffer[0]); // 255

buffer[1] = 256;
console.log(buffer[1]); // 0

buffer[2] = 260;
console.log(buffer[2]); // 4
console.log(buffer[2] === 260%256); // true

buffer[3] = 516;
console.log(buffer[3]); // 4
console.log(buffer[3] === 516%256); // true
```

If you try to assign a negative number to a byte,
it gets converted using the two’s complement system.

```js

buffer[4] = -50;
console.log(buffer[4]);  // 206
                         // -50(10) = 11001110(U2) = 256 + -50
parseInt('11001110', 2); // 206
```

## Multi-byte characters

There are many UTF-8 characters that take more than just one byte:
> Hello 🌎 world!

There is an emoji in the middle that consists of four bytes: `11110000 10011111 10001100 10001110`.

Let’s save this data in multiple buffers:

```js
const buffers = [
  Buffer.from('Hello '),
  Buffer.from([0b11110000, 0b10011111]),
  Buffer.from([0b10001100, 0b10001110]),
  Buffer.from(' world!'),
];
```

Something like this can happen for example when you are interpreting a big text file. If you parse it in chunks, one of the chunks might contain just a part of the character, just like above.

Let’s try to stringify it:

```js
let result = '';
buffers.forEach((buffer) => {
  result += buffer.toString();
});

console.log(result); // Hello ��� world!
```

To overcome the issue, use `StringDecoder`. It provides an API for decoding `Buffer` objects into strings while preserving multi-byte characters.


```ts
import { StringDecoder } from 'string_decoder';

const decoder = new StringDecoder('utf8');

const buffers = [
  Buffer.from('Hello '),
  Buffer.from([0b11110000, 0b10011111]),
  Buffer.from([0b10001100, 0b10001110]),
  Buffer.from(' world!'),
];

const result = buffers.reduce((result, buffer) => (
  `${result}${decoder.write(buffer)}`
), '');

console.log(result); // Hello 🌎 world!
```

The `StringDecoder` ensures that the decoded string does not contain any incomplete multibyte characters by holding the incomplete character in an internal buffer until the next call to the  `decoder.write()`.


