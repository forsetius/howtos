# PHP

## Różne wersje PHP na jednym hoście

1. Dodać repozytorium Ondreja Sury'ego:
```sh
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
```

2. Wejść w `aptitude` i zainstalować pożądane wersje PHP (np. 5.6 i 7.2) oraz potrzebne moduły
3. Upewnić się, że domyślną wersją PHP jest 5.6:
```sh
update-alternatives --set php /usr/bin/php5.6
```
4. W plikach virtual hostów, które tego potrzebują dopisać
```apache
Include "conf-available/php7.2-fpm.conf"
```
5. Restart Apache'a
```sh
sudo service apache2 restart
```
