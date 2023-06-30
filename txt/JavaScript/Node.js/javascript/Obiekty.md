## Obiekty

Przechowują **nie**uporządkowane zestawy danych oznaczonych kluczami typu _string_. Silnik V8 nie gwarantuje, że iteracja po danych będzie się odbywała w kolejności dodawania danych.
```js
var obj = { has_thing: true, id: 123 };
obj.newElem = 'abc'; // dodawanie nowego elementu
```
