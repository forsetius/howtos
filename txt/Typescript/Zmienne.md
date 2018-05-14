# Zmienne

- [Zmienne](#zmienne)
    - [Widoczność](#widocznos)
    - [Zmienność](#zmiennos)
    - [Przesłanianie](#przesanianie)
    - [Nagłówki pętli](#nagowki-ptli)
- [Destrukturyzacja](#destrukturyzacja)
    - [Destrukturyzacja tablic](#destrukturyzacja-tablic)
    - [Destrukturyzacja obiektów](#destrukturyzacja-obiektow)
        - [Zmiana nazwy właściwości obiektu](#zmiana-nazwy-wasciwosci-obiektu)
        - [Wartości domyślne](#wartosci-domyslne)
    - [Deklaracje funkcji](#deklaracje-funkcji)
- [Spread](#spread)

Zmienne i stałe należy najpierw zadeklarować a dopiero później można ich używać.

## Widoczność
- **var** - zmienna widoczna w całym bloku funkcji
```js
function func() {
    if (true) {
        var tmp = 123;
    }
    console.log(tmp); // 123
}
```

- **let** i **const** - zmienna widoczna w danym bloku instrukcji (for, if...)
```js
function func() {
    if (true) {
        let tmp = 123;
    }
    console.log(tmp); // ReferenceError: tmp is not defined
}
```

## Zmienność
Zmienne zadeklarowane przez `var` i `let` mogą mieć zmienianą wartość. Zmienne zadeklarowane przez `const` po wstępnym przypisaniu wartości nie mogą być już zmieniane.

Jednak jeśli do zmiennej zadeklarowanej przez `const` przypisany jest obiekt to stała jest tylko referencja do niego, sam stan obiektu można zmieniać. Aby temu zapobiec, trzeba go zamrozić:
```js
const obj = Object.freeze({});
obj.prop = 123; // TypeError
```

Tak samo jest w przypadku przypisania do takiej zmiennej tablicy.

## Przesłanianie
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

## Nagłówki pętli
`var` w pętli przypisuje zmiennej jedną, stałą referencję na całą pętlę:
```js
let arr = [];
for (var i=0; i < 3; i++) {
    arr.push(() => i);
}
console.log(arr.map(x => x())); // [3,3,3]
```

Zmienne zadeklarowane przez `let`/`const` przypisują zmiennej nową referencję dla każdej iteracji pętli
```js
let arr = [];
for (let i=0; i < 3; i++) {
    arr.push(() => i);
}
console.log(arr.map(x => x())); // [0,1,2]
```

# Destrukturyzacja

## Destrukturyzacja tablic
Najprostsza forma to przypisanie destrukturyzacyjne tablic:
```ts
let input = [1, 2];
let [first, second] = input;
console.log(first);             //  1
console.log(second);            //  2
```

Działa przy parametrach funkcji:

```ts
function f([first, second]: [number, number]) {
    console.log(first);         //  1
    console.log(second);        //  2
}
f([1, 2]);
```

Można utworzyć zmienne tylko dla kilku pierwszych wartosci w tablicy a resztę (opcjonalnie) umieścić w innej zmiennej:
```ts
let [first, ...rest] = [1, 2, 3, 4];
console.log(first);             //  1
console.log(rest);              //  [ 2, 3, 4 ]
```

Można też ostatnie lub niektóre elementy pominąć: 
```ts
let [first] = [1, 2, 3, 4];
console.log(first);             //  1
let [, second, , fourth] = [1, 2, 3, 4];
```

## Destrukturyzacja obiektów
```ts
let o = { a: "foo", b: 12, c: "bar" };
let { a, b } = o;
// tworzy zmienne a i b, o.c jest pomijane
```

Przypisanie bez deklaracji. trzeba dodać nawiasy, bo normalnie JS uznaje `{` za otwarcie bloku:
```ts
({ a, b } = { a: "baz", b: 101 }); 
```

Stworzenie zmiennej dla pozostałych właściwości:
```ts
let { a, ...passthrough } = { a: "foo", b: 12, c: "bar" };
let total = passthrough.b + passthrough.c.length;
```

### Zmiana nazwy właściwości obiektu
```ts
let { a: newName1, b: newName2 } = o;

// Równoważne:
let newName1 = o.a;
let newName2 = o.b;
```

W powyższym zapisie `:` służy oznaczeniu aliasów, nie typów. Aby oznaczyć typy:
```ts
let { a, b }: { a: string, b: number } = o;
```

### Wartości domyślne
Można przypisywać wartości domyślne na wypadek, gdyby wartość nie była ustawiona:

```ts
function keepWholeObject(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject;
}
```

## Deklaracje funkcji
Destrukturyzacja działa również w deklaracjach funkcji:
```ts
type C = { a: string, b?: number }
function f({ a, b }: C): void {
    // ...
}
```

Destrukturyzacja w deklaracjach funkcji w połączeniu ze zmianą nazw parametrów czy wartościami domyślnymi jest niewskazana - bardzo szybko rośnie stopień komplikacji.

# Spread
Odwrotność destrukturyzacji. Pozwala rozwinąć tablicę w innej tablicy:
```ts
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];  // [0, 1, 2, 3, 4, 5]
```
Spread tworzy płytkie kopie tablic `first` i `second` - nie są one mutowane.

Spread działa też dla obiektów
```ts
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { ...defaults, food: "rich" }; // { food: "rich", price: "$$", ambiance: "noisy" }
```

Ale należy pamiętać, że spread działa od lewej do prawej, więc następne właściwości zastępują poprzednie o tej samej nazwie. Poza tym przy robieniu spreadu na obiektach traci się metody i parametry typów z funkcji generycznych.
