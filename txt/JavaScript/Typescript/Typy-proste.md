# Typy proste

- [Typy proste](#typy-proste)
    - [Boolean](#boolean)
    - [Number](#number)
    - [String](#string)
    - [Array](#array)
    - [Tuple](#tuple)
    - [Enum](#enum)
    - [Any](#any)
    - [Void](#void)
    - [Null i Undefined](#null-i-undefined)
    - [Never](#never)
- [Asercje typów](#asercje-typow)

## Boolean
Typ logiczny. Wartości `true` i `false`.

```ts
let isDone: boolean = false;
```

## Number
W Typescripcie wszystkie liczby są zmiennoprzecinkowe. Oprócz reprezentacji dziesiętnej, TS oferuje inne reprezentacje:

```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

## String
Typ przechowujący ciągi znaków ograniczonych `"`, `'` lub `` ` ``. Te ostatnie oznaczają ***template strings***, które umożliwiają zagnieżdżanie wyrażeń w tekście i obsługują tekst wielo-liniowy.

```ts
let fullName: string = `Bob Bobbington`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ fullName }.

I'll be ${ age + 1 } years old next month.`;

// Ekwiwalent:
let sentence: string = "Hello, my name is " + fullName + ".\n\n" +
    "I'll be " + (age + 1) + " years old next month.";
```

## Array
Tablice znaków. W TS muszą mieć oznaczony typ.

```ts
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

## Tuple
Krotki - rodzaj tablic, które mają zdefiniowaną liczbę eementów, z których każdy ma przypisany typ.

```ts
// Deklaracja krotki
let x: [string, number];

// Inicjalizacja:
x = ["hello", 10]; // OK
x = [10, "hello"]; // błąd
```

Przy dostępie do zadeklarowanych elementów używane są ich zadeklarowane typy. Przy dostępie do niezadeklarowanych elementów używany jest ich typ składany (***union type***)

```ts
x[3] = "world"; // OK, 'string' może być przypisany do 'string | number'
x[3] = true;    // Błąd, 'string' nie pasuje do 'string | number'
```

## Enum
Typ wyliczeniowy. Umożliwia przypisanie "etykiet" do indeksów numerycznych.

```ts
enum Color {Red, Green, Blue};             // Red = 0, Green = 1, Blue = 2
enum Color {Red = 1, Green, Blue};         // Red = 1, Green = 2, Blue = 3
enum Color {Red = 1, Green = 2, Blue = 4}; // Red = 1, Green = 2, Blue = 4

// Inicjalizacja
let c: Color = Color.Green;

// Pobranie etykiety dla podanej wartości
let colorName: string = Color[2]; // 'Green' przy deklaracji z ostatniej linii
```

## Any
Oznaczenie na dowolny typ. **Przypisanie zmiennej typu `any` wyłącza sprawdzanie typów**. 

```ts
let list: any[] = [1, true, "free"];
list[1] = 100;

let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; 
```

## Void
Oznaczenie na brak typu. Używane, gdy funkcja nic nie zwraca

```ts
function warnUser(): void {
    alert("This is my warning message");
}
```
Do zmiennych typu `void` mogą zostać przypisane jedynie wartości `null` i `undefined`.

## Null i Undefined
Do typu `null` można przypisać tylko `null`.
Do typu `undefined` można przypisać tylko `undefined`.

```ts
let u: undefined = undefined;
let n: null = null;
```

Tych dwu typów dotyczy flaga kompilatora `--strictNullChecks` - jeśli jest ustawiona to `null` i `undefined` można przypisać tylko do `void`. Jeśli nie, to można przypisać `null` i `undefined` do każdego innego typu, np. `number`.

## Never
Oznaczenie dla zmiennych i typów zwrotnych, których nie można uzyskać, np:

- funkcja nigdy nic nie zwraca, bo zawsze wyrzuca wyjątek
- zmienna nigdy nie uzyskuje typu, bo czujka typów (***type guard***) obcina możliwe do uzyskania typy do zera

# Asercje typów
Sposób na wymuszenie określonego typu. Informacja dla kompilatora, który nie sprawdza typu tulko przyjmuje za pewnik podany mu typ.

Są dwie formy:
```ts
// angle-bracket
let strLength: number = (<string>someValue).length;

// as
let strLength: number = (someValue as string).length;
```
