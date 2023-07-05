# Apt-cacher-ng

> _Serwer pośredniczący pakietów_

**Cache pakietów** jest przydatny, jeśli w sieci domowej mamy kilka komputerów z systemami Linux używającymi systemu pakietów APT. Najlepiej, jeśli komputery używają tej samej dystrybucji. Dzięki temu rozwiązaniu aktualizacje pakietów są z Internetu pobierane raz, a następne komputery uaktualniają się juz z lokalnego cache. Jest to szybsze i nie angażuje tak mocno połączenia z Internetem.

## Instalacja
```shell
sudo apt-get install apt-cacher-ng
```

## Konfiguracja

Na każdym kliencie tworzymy plik `/etc/apt/apt.conf.d/02proxy`:
```shell
cd /etc/apt/apt.conf.d
sudo cp 00aptitude 02proxy
```

w pliku `02proxy` zmienić zawartość na:
```
Acquire::http { Proxy "http://192.168.1.40:3142"; };
```
