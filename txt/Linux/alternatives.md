# System Linux

## Alternatywy

Można zainstalować różne wersje niektórych pakietów (np. PHP 5.6, 7.1 i 7.2) ale zawsze któraś będzie domyślna. Ustawienia dokonuje się przez `update-alternatives`:

`sudo update-alternatives --list php` - pokaż wszystkie dostępne pliki wykonywalne PHP
`sudo update-alternatives --display php` - pokaż wszystkie dostępne wersje PHP z priorytetami
`sudo update-alternatives --set php /usr/bin/php5.6` - ustaw PHP 5.6 jako domyślny
`sudo update-alternatives --auto php` - ustaw automatycznie wersję PHP (zwykle do najwyższej wersji)
