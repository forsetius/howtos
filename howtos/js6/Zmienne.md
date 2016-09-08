# Javascript ECMAScript 6

## Zmienne
Zmienne i stałe należy najpierw zadeklarować a dopiero później można ich używać.

#### Widoczność
- **var** - zmienna widoczna w całym bloku funkcji
```js
    function func() {
        if (true) {
            let tmp = 123;
        }
        console.log(tmp); // ReferenceError: tmp is not defined
    }
```
- **let** - zmienna widoczna w danym bloku instrukcji (for, if...)
```js
function func() {
    if (true) {
        var tmp = 123;
    }
    console.log(tmp); // 123
}
```
- **const** - stała widoczna w danym bloku instrukcji (for, if...)

#### Zmienność
Zmienne zadeklarowane przez `var` i `let` mogą mieć zmienianą wartość. Stałe zadeklarowane przez `const` po wstępnym przypisaniu wartości nie mogą być już zmieniane.

Jednak jeśli do stałej przypisany jest obiekt to stała jest tylko referencja do niego, sam stan obiektu można zmieniać. Aby temu zapobiec, trzeba go zamrozić:
```js
const obj = Object.freeze({});
obj.prop = 123; // TypeError
```

#### Przesłanianie
Zmienne zadeklarowane przez `var` (widoczne w całym bloku funkcji) mogą być przesłaniane przez zmienne zadeklarowane przez `let`.

```js
function func() {
    let foo = 5;
    if (···) {
        let foo = 10; // shadows outer `foo`
        console.log(foo); // 10
    }
    console.log(foo); // 5
}
```

#### Nagłówki pętli
Stałe i zmienne zadeklarowane przez `var` wiążą zmienną dla całej pętli
```js
let arr = [];
for (var i=0; i < 3; i++) {
    arr.push(() => i);
}
console.log(arr.map(x => x())); // [3,3,3]
```

Zmienne zadeklarowane przez `let` dają nowe wiązanie dla każdej iteracji pętli
```js
let arr = [];
for (let i=0; i < 3; i++) {
    arr.push(() => i);
}
console.log(arr.map(x => x())); // [0,1,2]
```
