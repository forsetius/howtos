# Interfejsy

- [Interfejsy](#interfejsy)
    - [Podstawowa postać interfejsu](#podstawowa-posta-interfejsu)
    - [Właściwości opcjonalne](#wasciwosci-opcjonalne)
    - [Właściwości tylko do odczytu](#wasciwosci-tylko-do-odczytu)
    - [Kontrola nadmiarowych właściwości](#kontrola-nadmiarowych-wasciwosci)
    - [Typy funkcji](#typy-funkcji)
    - [Typy indeksowane](#typy-indeksowane)
    - [Typy klasowe](#typy-klasowe)
        - [Implementacja interfejsu](#implementacja-interfejsu)
        - [Różnica między stronami: statyczną i instancyjną klasy](#ronica-midzy-stronami--statyczn-i-instancyjn-klasy)
    - [Rozszerzanie interfejsów](#rozszerzanie-interfejsow)
    - [Typy hybrydowe](#typy-hybrydowe)
    - [Interfejsy rozszerzające klasy](#interfejsy-rozszerzajce-klasy)

## Podstawowa postać interfejsu
Interfejsy w TS są rodzajem kontraktu, który przypisuje nazwę do zdefiniowanej struktury. Sprawdzanie zgodności typów odbywa się przez pporównanie struktur, nie nazw.

Przekształcenie typu opisanego _inline_ na interfejs:
```ts
// typ zdefiniowany inline
function printLabel(labelledObj: { label: string }) {
    console.log(labelledObj.label);
}

// typ zdefiniowany interfejsem
interface LabelledValue {
    label: string;
}

function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label);
}

// wykorzystanie
printLabel({size: 10, label: "Size 10 Object"});
``` 

Kontrola typów sprawdza, czy w wywołaniu `printLabel` przekazano obiekt, który ma **co najmniej** właściwość `label` typu `string`. Nie trzeba oznaczać, że przekazywany obiekt implementuje `LabelledValue` - kompilator sprawdza kształt, nie nazwę.

## Właściwości opcjonalne
W interfejsach można ujmować również właściwości opcjonalne. Umożliwia to opisanie pełnej postaci typu danych, wszystkich właściwości, jakie mogą wystąpić, choć nie są wymagane - równocześnie nie dopuszczając żadnych nieprzewidzianych właściwości. Pozwala to zrealizować wzór "worka opcji" (***option bag***), w którym do funkcji wprowadza się obiekt, w którym ustawione są tylko właściwości o niestandardowych wartościach.

Właściwości opcjonalne oznacza się `?:` przed nazwą typu:
```ts
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): {color: string; area: number} {
    let newSquare = {color: "white", area: 100};
    if (config.color) { newSquare.color = config.color; }
    if (config.width) { newSquare.area = config.width * config.width; }
    return newSquare;
}

let mySquare = createSquare({color: "blue"}); // {color: 'blue'; area: 100}
```

## Właściwości tylko do odczytu
Słowo kluczowe `readonly` pozwala zadeklarować, że oznaczona nim właściwość może być ustawiona tylko raz, przy tworzeniu obiektu:
```ts
interface Point {
    readonly x: number;
    readonly y: number;
}

let p1: Point = { x: 10, y: 20 };
p1.x = 5; // błąd!
```

Jeśli potrzebne jest zabezpieczenie tablic przed zmianą, należy wykorzystać `ReadonlyArray<T>` - jest to wersja `Array<T>`, z której usunięto wszystkie metody modyfikujące:
```ts
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
```

Podobnie jak w przypadku `const`, zabezpieczana przed ponowną zmianą są właściwości (czy też poszczególne elementy tablicy) ale jeśli są one obiektami czy zwykłymi tablicami to w dalszym ciągu są one mutowalne. Chronione są tylko referencje.

## Kontrola nadmiarowych właściwości
TODO

## Typy funkcji
Za pomocą interfejsów można również opisywać postać funkcji:
```ts
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    let result = source.search(subString);
    return result > -1;
}
```

Nazwy argumentów nie muszą się zgadzać między interfejsem a implementacją, ważne są ilość, pozycja i typy. W zasadzie w implementacji funkcji nie trzeba już podawać ani typów argumentów ani typu zwrotnego funkcji. Jeśli się je opuści, zostaną wywnioskowane z deklaracji interfejsu.

## Typy indeksowane


## Typy klasowe

### Implementacja interfejsu

### Różnica między stronami: statyczną i instancyjną klasy

## Rozszerzanie interfejsów

## Typy hybrydowe

## Interfejsy rozszerzające klasy


