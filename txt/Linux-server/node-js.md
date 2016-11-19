# Node.js

Ponieważ node.js obecny w repozytoriach Debiana czy Ubuntu jest raczej przestarzały, instalujemy go z archiwum ściągniętego ze strony https://nodejs.org. Aby było bezpiecznie i łatwo się aktualizowało, instalujemy w `~/.local/opt`.

## Instalacja

1. Ściągamy archiwum z https://nodejs.org/en/
2. Rozpakowujemy przez `mc` w `~/.local/opt`
3. Robimy symlinka z `~/.local/opt/node-vX.X.X-linux-x64/bin/node` do  w `~/.local/bin/node`
4. Robimy symlinka z `~/.local/opt/node-vX.X.X-linux-x64/bin/npm` do  w `~/.local/bin/npm`
5. Test: `node -v`. Jeśli nie znajduje polecenia to trzeba upewnić się, że w `~/.profile` katalog `~/.local/bin` jest dodany do ścieżki:
  ```sh
  PATH="$HOME/.local/bin:$PATH"
  ```

Już.

## Użytkowanie
Moduły zainstalowane globalnie przez:
```sh
npm install -g <module>
```
pojawią się w `~/.local/opt/node-vX.X.X-linux-x64/lib/node_modules/<module>`. Po zainstalowaniu trzeba zrobić symlinki z ich katalogów `bin/` do `~/.local/opt/node-vX.X.X-linux-x64/bin/`.
