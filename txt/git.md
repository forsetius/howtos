# Git

## Setup

Sprawdzenie czy zainstalowany:
```bash
git
```

Założenie lokalnego repozytorium w danym katalogu:
```bash
git init
```

Sklonowanie repozytorium czyli rozpoczęcie pracy - założenie lokalnego repo i ściągnięcie plików. Tworzy połączenie (remote) ze zdalnym repozytorium o nazwie "origin". Jeśli nie podano `dir_name` to repo jest tworzone w katalogu o nazwie wziętej z nazwy repozytorium.
```bash
git clone <repo> [dir_name]
```

Formaty definicji repozytorium:
- `ssh://[user@]host.xz[:port]/path/to/repo.git/`
- `git://host.xz[:port]/path/to/repo.git/`
- `http[s]://host.xz[:port]/path/to/repo.git/`
- `ftp[s]://host.xz[:port]/path/to/repo.git/`
- `rsync://host.xz/path/to/repo.git/`
- `[user@]host.xz:path/to/repo.git/`
- `file:///path/to/repo.git/`
- `/path/to/repo.git/`

## Workflow
![Git workflow](../img/git-workflow.png)

## Używanie lokalnego repozytorium
Sprawdzenie statusu plików:
```bash
git status
```

Dodanie wszystkich plików w danym katalogu i podkatalogach do staging area:
```bash
git add .
```

Wysłanie plików ze staging area do lokalnego repozytorium:
```bash
git commit -m "Opis commita"
```

Wysłanie plików z bieżącego katalogu prosto do lokalnego repozytorium:
```bash
git commit -a -m "Opis commita"
```

Poprawienie opisu ostatniego commita:
```bash
git commit -m "opis"    # tu daliśmy zły opis`
git commit --amend      # jeśli nie dodaliśmy w międzyczasie żadnych plików to tak zmieniamy opis
```

Dodanie pliku do ostatniego commita:
```bash
git commit -m "opis"    # tu zapomnieliśmy o jakimś pliku
git add plik.txt        # dodajemy plik
git commit --amend      # nowy commit zawierający plik.txt zastępuje poprzedni commit
```

# Tagowanie

Dodanie taga do ostatniego commita:
```bash
git tag -a -m "Opis Taga" <tag>
```

Wysłanie taga na zdalny serwer:
```bash
git push <remote> --tags
```

## Undo

Usunięcie plików ze staging area (nie będą commitowane):
```bash
git reset HEAD [<file>]
git unstage [<file>]      # alias
```

Wyjęcie pliku spod wersjonowania - plik przestanie być śledzony przez Gita:
```bash
git rm --cached <file>
git unversion <file>      # alias
```

Wycofanie commita i pozostawienie nienaruszonego stage'a i plików. Cofnięty jest sam fakt scommitowania:
```bash
git reset --soft HEAD~1
git uncommit               # alias
```

TODO Zresetowanie commita lokalnego (przed wysłaniem na zdalny serwer) do stanu zachowanego w repozytorium zdalnym:


## Używania zdalnego repozytorium:

### Przygotowania
Określenie tożsamości użytkownika
```bash
git config --global user.email "forseti.pl@gmail.com"
git config --global user.name "Marcin FORSETI Paździora"
```

Konfiguracja jest zapisywana w pliku [~/.gitconfig](../file/gitconfig).

Zdefiniowanie zdalnego repozytorium (remote, np. `origin`) w systemie lokalnym
```bash
git remote add <nazwa-połączenia> <adres-zdalnego-repozytorium>
```

### Używanie

Skopiowanie zawartości zdalnego repozytorium lokalnie
```bash
git pull <nazwa-połączenia> <nazwa-gałęzi>
```

Wysłanie plików z lokalnego do zdalnego repozytorium
```bash
git push -u <nazwa-połączenia> <nazwa-gałęzi>
```

Przywrócenie commita w zdalnym repozytorium. Nie kasuje pośrednich commitów tylko kopiuje żądany commit na koniec historii.
```bash
git revert <commit>
git revert HEAD~1      # przywróć przedostatni commit
```
