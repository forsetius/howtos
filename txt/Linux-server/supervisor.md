# Supervisor

**Supervisor** jest usługą nadzorującą stan innych procesów. Używa się jej do zapewnienia, że zadane procesy są ciągle uruchomione. Jeśli proces zakończy się, **_supervisor_** uruchamia go ponownie.

## Instalacja
```sh
apt-get install supervisor
```

## Konfiguracja
Plik konfiguracyjny **_supervisora_** to `/etc/supervisor/supervisord.conf`. Definicje procesów do nadzorowania umieszcza się w katalogu `/etc/supervisor/conf.d/` w plikach `*.conf`. Zwykle wystarczy wpis:
```ini
[program:verdaccio]
command=/home/worker/.local/bin/verdaccio
autostart=true
autorestart=true
user=worker
environment=PATH="$PATH:/home/worker/.local/bin"
```

Prawidłowe ustawienie PATH jest szczególnie ważne dla skryptów, które korzystają z `/usr/bin/env` - m.in. skrypty PHP i Node.js.

## Użycie
Kontrola usługi **_supervisor_** odbywa się standardowo:
```sh
sudo service supervisor start|stop|restart|status
```

Jeśli chcemy zarządzać nadzorowanymi procesami to trzeba użyć `supervisorctl`. W razie zmian w konfiguracji wprowadza się je przez:
```sh
supervisorctl reread
supervisorctl update
```

Wydanie polecenia `supervisorctl` powoduje wypisanie statusów nadzorowanych procesów i wejście w tryb interaktywny, w którym można nimi zarządzać. W trybie tym działają m.in. polecenia: `start`, `stop`, `restart`, `tail`:
```sh
supervisor> start verdaccio
```

`tail` pozwala podejrzeć ostatnie kilka linijek wyjścia `stdout` i `stderr`:
```sh
supervisor> tail verdaccio
supervisor> tail verdaccio stderr
```

Oprócz tego dostępne są polecenia ogólne: `status`,  `help` i `quit`.
