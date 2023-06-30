# composer

## Instalacja
Composera najlepiej instalować w `~/.local/bin` - w ten sposób unikamy ceregieli przy jego aktualizacji.
Instalacja za pomocą [skryptu instalacyjnego](composer-installer.sh).
Po wszystkim warto jeszcze stworzyć symlinka z `composer.phar` do `composer`.

Sprawdzenie, czy działa: `composer -V`.

## composer.json

W głównym katalogu projektu trzeba stworzyć plik konfiguracyjny dla projektu.

**Przykładowe szablony**:
* [dla projektu Symfony](symfony-project-composer.json)
* [dla biblioteki Symfony](../../symfony-library-composer.json)

**Klucze**:
- `name` - nazwa projektu w formacie `vendor/project`
- `type` - rodzaj paczki: `project` - projekt, `library` - biblioteka, którą Composer skopiuje do vendor/
- `licence` - licencja, np: `proprietary` - własnościowy, closed source, `MIT` itd.
- `autoload` - definicje autoloadingu, dla Symfony:
  ```json
  {
    "autoload": {
      "psr-4" : {
        "" : "src/",
        "Forseti\\SfCommons\\AnyBundle" : ""
      }
    }
  }
  ```
  1. autoloader ładuje z `src/` i tamże powinna zaczynać się hierarchia katalogów odpowiadająca przestrzeniom nazw. Np. `Forseti\\SfCommons\\AdminBundle\\Controller\\UserController` powinien znajdować się w pliku `src/Forseti/SfCommons/AdminBundle/Controller/UserController.php`.

  2. główny katalog jest zmapowany do `Forseti\\SfCommons\\AnyBundle`, więc wszystkie klasy w głównym katalogu powinny należeć do tej przestrzeni nazw. Np. `Forseti\\SfCommons\\AnyBundle\\Controller\\AnyController` powinien znajdować się w `Controller/AnyController.php`.
- `repositories` - dodatkowe repozytoria poza Packagistem:
  ```json
  {
    "repositories": [
      {"type": "composer", "url": ""}
      {"type": "vcs", "url": "git@git.forseti.pl:forseti-sf/helper.git"}
      {"type": "package", "package": {
          "name": "smarty/smarty",
          "version": "3.1.7",
          "dist": {
              "url": "http://www.smarty.net/files/Smarty-3.1.7.zip",
              "type": "zip"
          },
          "source": {
              "url": "https://smarty-php.googlecode.com/svn/",
              "type": "svn",
              "reference": "tags/Smarty_3_1_7/distribution/"
          }
        }
      }
    ]
  }
  ```
  1. repozytorium Composera
  2. projekt w systemie kontroli wersji
  3. pojedyncza paczka - trzeba podać tutaj całą definicję paczki, bo może nie mieć swojego `composer.json`
- `require` i `require-dev` - zależności potrzebne w środowisku produkcyjnym i dodatkowe potrzebne w środowisku deweloperskim. Dla każdej zależności podaje się wpis: `"vendor/package-name": "version"`.

##  Oznaczenia wersji:
  - `"1.2.3"` - dokładnie wersja `1.2.3`
  - `"^1.2.3"` - wersja nie mniejsza niż `1.2.3` i mniejsza niż `2.0`
  - `"~1.2.3"` - wersja nie mniejsza niż `1.2.3` i mniejsza niż `1.3`
  - `dev-<nazwa-brancha>` - HEAD gałęzi `nazwa-brancha`, np. `dev-new-features`
  - `<nazwa-brancha>-dev`- jeśli `nazwa-brancha` wygląda jak nazwa taga (np. `v1`) to używamy tej składni. Jeśli nazwa gałęzi nie zawiera liter to składnia jest: `<nazwa-brancha>.x-dev`
  - `"*"` - dowolna wersja

Dodatkowo powinno się podać wymaganą wersję PHP i jego rozszerzeń:
```json
{
  "php" : ">=5.6",
  "ext-curl" : "*"
}
```
Listę zainstalowanych rozszerzeń można uzyskać przez `composer show --platform`
-
