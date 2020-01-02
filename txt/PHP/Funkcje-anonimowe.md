# Funkcje anonimowe - _Closures_

Funkcje bez nazwy. Stosowane gdy potrzebna jest funkcja wykorzystywana tylko w jednym miejscu.

**Użycie**:
```php
<?php
echo preg_replace_callback(
  '~_([a-z])~',
  function ($match) {
    return strtoupper($match[1]);
  },
  'hello_world'
);
// outputs helloWorld
?>
```

Od wersji 7.4 można pisać krócej:
```php
<?php
echo preg_replace_callback(
  '~_([a-z])~',
  fn($match) => strtoupper($match[1]),
  'hello_world'
);
// outputs helloWorld
?>
```

Closures można przypisywać do zmiennej:
```php

<?php
$greet = fn($name) => strtoupper($match[1]);

$greet('World');
$greet('PHP');
?>

```

Closures mogą dziedziczyć zmienne z parent scope ale muszą one być wprowadzone przez _use_:
```php
<?php
$message = 'hello';

$example = function () use ($message) {
    var_dump($message);
};
$example();
```
Zmienne przekazywane przez _use_ nie mogą być superglobalami, $this ani zmiennymi o nazwie takiej samej jak parametr closure. Zmienne te mają wartość z momentu, gdy closure była definiowana, **nie kiedy jest wywoływana**. Aby wartość zmiennej w closure była taka jak przy wywołaniu, musi być dziedziczona przez referencję:
```php
<?php
$message = 'hello';

$example = function () use (&$message) {
    var_dump($message);
};
$example();
```
