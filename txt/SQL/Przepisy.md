# Przepisy dla Postgresa

## Update wielu rekordów w jednym zapytaniu
We `FROM` tworzymy podzapytanie złożone z tablicy par (`id`, `val`). W `WHERE` trzeba powiązać rekord z aktualizowanej tabeli z elementem tablicy.

```sql
update up_route_laps as l
set payment_state = mt.state
from (values
    (200,1), (199,0), (197,1), (196,0), (195,1), 
    (194,2), (193,1), (192,1), (191,2), (190,2), 
    (189,2), (188,2), (187,1), (186,2), (185,0), 
    (184,0), (183,0), (182,0), (181,2), (180,0), 
    (179,1)
) as mt(id, state)
where mt.id = l.id
```

## Import CSV do istniejącej tabeli

```sql
COPY zip_codes 
FROM '/path/to/csv/ZIP_CODES.txt' 
WITH (FORMAT csv[, dodatkowe opcje CSV])
```

Dodatkowe opcje
``` 
    DELIMITER 'delimiter_character'
    NULL 'null_string'
    HEADER [ boolean ]
    QUOTE 'quote_character'
    ESCAPE 'escape_character'
    FORCE_QUOTE { ( column_name [, ...] ) | * }
    FORCE_NOT_NULL ( column_name [, ...] )
    FORCE_NULL ( column_name [, ...] )
    ENCODING 'encoding_name'
```

Jeżeli eksportujemy z PhpPgAdmina jako TSV a przy imporcie ustawimy `format` na CSV to trzeba najpierw ręcznie wyedytować w LibreOffice Calc (import z rozdzielaczem `tab` i eksport z rozdzielaczem `;` bez nagłówków). Następnie przy imporcie do Postgresa ustawić `delimiter` na `";"`, `null` na `"\n"` a `header` na `false`.