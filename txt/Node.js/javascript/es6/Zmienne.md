# Javascript ECMAScript 6

- [Zmienne](#zmienne)
	- [Widoczność](#widoczno)
	- [Zmienność](#zmienno)
	- [Przesłanianie](#przesanianie)
	- [Nagłówki pętli](#nagwki-ptli)

## Zmienne
Zmienne i stałe należy najpierw zadeklarować a dopiero później można ich używać.

#### Widoczność
- **var** - zmienna widoczna w całym bloku funkcji
```js
function func() {
    if (true) {
        var tmp = 123;
    }
    console.log(tmp); // 123
}
```

- **let** - zmienna widoczna w danym bloku instrukcji (for, if...)
```js
function func() {
    if (true) {
        let tmp = 123;
    }
    console.log(tmp); // ReferenceError: tmp is not defined
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
Zmienne zadeklarowane w bloku nadrzędnym mogą być przesłaniane w blokach podrzędnych przez zmienne zadeklarowane przez `let`.

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
`var` w pętli przypisuje zmiennej jedną, stałą referencję na całą pętlę:
```js
let arr = [];
for (var i=0; i < 3; i++) {
    arr.push(() => i);
}
console.log(arr.map(x => x())); // [3,3,3]
```

Zmienne zadeklarowane przez `let` przypisują zmiennej nową referencję dla każdej iteracji pętli
```js
let arr = [];
for (let i=0; i < 3; i++) {
    arr.push(() => i);
}
console.log(arr.map(x => x())); // [0,1,2]
```
