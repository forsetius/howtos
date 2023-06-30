# Verdaccio

**Verdaccio** jest aplikacją implementującą prywatne repozytorium NPM z funkcją proxy oficjalnego repozytorium.

## Instalacja
Konieczne jest [zainstalowanie Node.js](./node-js.md).

Ze zwykłego użytkownika:
```sh
npm install -g verdaccio
```

## Konfiguracja
### Plik konfiguracyjny
Raz uruchamiamy:
```sh
verdaccio
```
i zaraz przerywamy `^c`. To utworzy plik konfiguracyjny `~/.config/verdaccio/config.yaml`, który trzeba wyedytować.

W pliku tym muszą się znaleźć dyrektywy:
```yaml
listen:
  - 51.255.169.26:9000 # Koniecznie trzeba podać port. Jeśli IP jest 0.0.0.0 to nasłuchuj na wszystkich IP

uplinks:
  npmjs:
    url: https://registry.npmjs.org/

packages:
  '@*/*':
    # scoped packages
    proxy: npmjs

  '**':
    proxy: npmjs

  web:
    enable: true
```

### Współżycie z innymi serwerami
Jeśli **_verdaccio_** ma być dostępny pod subdomeną to trzeba:
1. zdefiniować subdomenę w panelu u providera
  ```
  npm.forseti.pl. A 51.255.169.26
  ```
2. zrobić reverse proxy na serwerze HTTP, który obsługuje danego IP. Jeśli jest to nginx Gitlaba to trzeba:
  1. Stworzyć plik `/etc/nginx/vhost/verdaccio.conf`:
    ```
    server {
      listen: 0.0.0.0:80;
      server_name: npm.forseti.pl;
      server_tokens off;
      location / {
        proxy_pass 51.255.169.26:9000;
      }
    }
    server {
      listen: 0.0.0.0:80;
      server_name: git.forseti.pl;
      server_tokens off;
      return 302 https://git.forseti.pl:443$request_uri;
    }
    ```
  2. w pliku `/etc/gitlab/gitlab.rb` odkomentować `nginx[custom_nginx_config]` i ustawić na powyższy plik
  3. wykonać `gitlab-ctl reconfigure`
  4. w pliku `/var/opt/gitlab/nginx/conf/gitlab-http.conf` zakomentować całą sekcję:
    ```
    server {
      listen: 0.0.0.0:80;
      ...
    }
    ```
  5. wykonać `gitlab-ctl nginx restart`

### Rejestracja
Rejestracja repozytorium:
```sh
npm set registry http://51.255.169.26:9000
```

Rejestracja użytkownika:
```sh
npm adduser --registry=http://51.255.169.26:9000 --scope=@forseti-node
```

## Uruchamianie
Uruchamia się przez:
```sh
verdaccio
```
ale chcemy, żeby działało stale, więc używamy [supervisora](supervisor.md). Należy pamiętać o ustawieniu PATH.

## Użycie
### Publikowanie

Paczka do opublikowania musi zawierać plik [package.json](node-package.json).

```sh
npm pack <nazwa-katalogu-pakietu>
npm publish <nazwa-paczki-tgz> --access restricted
```
