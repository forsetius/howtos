# Process arguments

The process object is a property of the global object and holds information about the environment of our Node.js app.

```js
// main.js
process.argv.forEach((arg) => console.log(arg));
```

```
$ node ./main.js one two three

/usr/bin/node
/home/marcin/.../main.js
one
two
three
```
