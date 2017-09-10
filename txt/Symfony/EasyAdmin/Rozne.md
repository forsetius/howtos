# Różne

## Druga konfiguracja encji
Jeśli potrzeba kolejnego zestawu widoków dla encji to można zdefiniować kolejny wpis w `easy_admin/entities` z nową nazwą ale wskazujący na tą samą klasę i w nim zdefiniować kolejny zestaw widoków.

```yaml
easy_admin:
    entities:
        User:
            class: Admin/Entity/User
        BadUser:
            class: Admin/Entity/User
```

W tej nowej, wirtualnej encji można ustawić jakieś filtrowanie, inne sortowanie, odmienny zestaw pól itd.

## Zmiana szablonów
Szablony dla widoków można definiować zarówno globalnie, pod kluczem `easy_admin/design/templates` jak i per encja - pod kluczem `easy_admin/entities/<encja>/templates`

## Widok `form`
Widok `form` jest wykorzystywany przez widoki `new` i `edit`. Jeśli mają się one (nieznacznie) różnić to mimo wszystko nie trzeba definiować ich osobno lecz zdefiniować widok `form`, a np. w widoku `new` usunąć niepożądane pole:
```yaml
easy_admin:
    entities:
        User:
            form:
                fields:
                    - name
                    - email
            new:
                fields:
                    - '-id' # usuwamy pole `id`
```
Jako, że nie ma dobre metody na dodawanie pól w `new` i `edit` tak, żeby nadać im pożądany porządek, najlepiej w `form` dodać wszystkie pola, jakie mogą wystąpić w `new` i `edit` a w nich tylko odejmować niepotrzebne.
