## Tablice

Przechowują uporządkowane zestawy danych oznaczane indeksami. Indeksami mogą być liczby całkowite.
```js
var arr = [ 'a', 'b', 'c'];
arr.push('d');    // dodaj na końcu
arr.pop();        // usuń ostatni element
arr.unshift('1'); // dodaj na początku
arr.shift();      // usuń pierwszy element
```

### Przeszukiwanie
|   |   |
|---|---|
| `Array.isArray(array)` | Returns true if a variable is an array, false if it is not. |
| `indexOf(searchElement[, fromIndex])` |	Returns the first (least) index of an element within the array equal to the specified value, or -1 if none is found. The search can optionally begin at fromIndex.|
| `lastIndexOf(searchElement[, fromIndex])` |	Returns the last (greatest) index of an element within the array equal to the specified value, or -1 if none is found.The array is searched backwards, starting at fromIndex.

`indexOf` i `lastIndexOf` są przydatne do wyszukiwania pożądanej wartości w tablicy:
```js
function process(argv) {
  if(argv.indexOf('help')) {
    console.log('Found!');
  }
}
process(['foo', 'bar', 'help']); // zwraca: Found!
```

Jednak należy pamiętać, że funkcje te do porównywania używają `===`, więc sprawdzają zgodność typów:
```js
var arr = ["1", "2", "3"];
console.log(arr.indexOf(2));    // zwraca -1 (nie znaleziono)
console.log(arr.indexOf("2"));  // zwraca 1
console.log(arr.indexOf(2+'')); // zwraca 1 (po rzutowaniu na string)
```

### Wywoływanie funkcji dla każdego elementu tablicy

|   |   |
|---|---|
| `filter(callback[, thisObject])` |Creates a new array with all of the elements of this array for which the provided filtering function returns true. If a thisObject parameter is provided to filter, it will be used as the this for each invocation of the callback. IE9|
| `forEach(callback[, thisObject])` |	Calls a function for each element in the array.|
| `map(callback[, thisObject])` |	Creates a new array with the results of calling a provided function on every element in this array.|

Każda z funkcji `filter()`, `map()` i `forEach()` dla każdego z elementów tablicy wywołuje callback podany jako parametr.
