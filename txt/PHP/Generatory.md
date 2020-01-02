# Generatory

Generatory to funkcje, które generują wartości w określony sposób. Jego wywołanie zwraca obiekt, który może być iterowany jak tablica.

Generator wygląda jak zwykła funkcja ale zawiera słowo kluczowe _yield_, które zwraca wartość w danej iteracji ale nie wychodzi z funkcji tylko zawiesza jej wykonanie tak, że może być wznowione przy kolejnej iteracji.

Generator może zawierać słowo _return_, które przerywa jego wykonanie i ew. zwraca wartość, którą można pobrać przez `Generator::getReturn()`.

```php
<?php
function xrange($start, $limit, $step = 1) {
  for ($i = $start; $i <= $limit; $i += $step) {
    yield $i;
  }
}

foreach (xrange(1, 9, 2) as $number) {
  echo "$number ";
}
```

Aby zwrócić parę klucz => wartość należy użyć składni
```php
yield key => value;
```

Aby zwrócić wartość przez referencję:
```php
<?php
function &gen($value) {
  while ($value > 0) {
    yield $value;
  }
}
```

Zabezpieczanie przed wyciekiem zasobów:
```php
<?php
function generator() {
  $complete = false;
  try {
    while (($result = some_function())) {
      yield $result;
    }
    $complete = true;
  } finally {
    if (!$complete) {
      // cleanup when loop breaks
    } else {
      // cleanup when loop completes
    }
  }

  // Do something only after loop completes
}
?>
```
