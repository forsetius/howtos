# Filesystem

The fs module gives us an API to interact with the file system. All operations have synchronous and asynchronous forms. The asynchronous function always takes a callback as its last argument.

## `writeFile`

```ts
import * as fs from 'fs';
import * as util from 'util'

const writeFile = util.promisify(fs.writeFile); // turns callbacks into Promises

writeFile('./newFile.txt', null) // args: file name, file contents
  .then(() => { console.log('File created successfully'); })
  .catch(error => { console.log(error); });
```

## `readFile`
The `readFile` command reads the file that is under that path. The second argument of the `readFile` function is an object with additional options. We use it to define the encoding of a file. Without it, the readFile function results with a `Buffer`.

```ts
import * as fs from 'fs';
import * as util from 'util'

const readFile = util.promisify(fs.readFile);

async function cat(path: string) {
  try {
    const content = await readFile(path, { encoding: 'utf8' });
    console.log(content);
  } catch (error) {
    console.log(error);
  }
}
```
