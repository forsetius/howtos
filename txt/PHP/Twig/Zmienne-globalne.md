# Zmienne globalne Twig
Symfony automatycznie dodaje zmienne globalne Twiga jeśli są odpowiednio skonfigurowane.

## Konfiguracja statyczna w plikach konfiguracyjnych
W dowolnym pliku konfiguracyjnym trzeba ustawić klucz `twig.globals`:
```yaml
twig:
    globals:
        variableName: value
```

Jeśli potrzebujemy tej wartości również poza Twigiem to ustawiamy ją jako parametr i linkujemy w sekcji `twig`:
```yaml
parameters:
    variableName: value

twig:
    globals:
        variableName: "%variableName%"
```

## Konfiguracja dynamiczna przez usługę
