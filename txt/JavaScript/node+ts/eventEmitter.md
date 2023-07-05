# EventEmitter

Certain objects can emit events and we call them **emitters**. We can listen to those and react in a way using callback functions called **listeners**.

An instance of the `EventEmmiter` has a 3 methods of main interest:
1. `on(eventName, callbackFn)` - allows one or more functions to be attached to an object. The function(s) get called when an event of `eventName` type is emitted. The functions are called **synchronously**, in order they were added.
2. `once(eventName, callbackFn)` - as above but such function(s) will be called only once for `eventName` type of event
3. `emit(eventName, data)` - causes an `eventName`-type event to be emitted. Only already registered listener function get called. Optionally, second argument `data` can be used to pass some additional data to the listeners.

```js
import * as EventEmitter from 'events';

const eventEmitter = new EventEmitter();

eventEmitter.on('event', function() {
  console.log('one');
});
eventEmitter.on('event', function() {
  console.log('two');
});
eventEmitter.on('event', function() {
  console.log('three');
});

eventEmitter.emit('event');
```

Things to remember of:
- the `on` function is an alias for `addEventListener` and both functions act the same way
- the `EventEmitter` calls all of the above functions synchronously
- if a listener is attached after the `emit` function is called, the `EventEmitter` does not call it
- inside of a listener function, the `this` keyword refers to the instance of the `EventEmitter`. If you use the arrow functions there, the `this` keyword no longer references to the Node.js `EventEmitter` instance.


