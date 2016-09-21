# SSH HOWTO

## Klient
**Połączenie**:
`ssh user@http://192.168.0.1 -p 22`
gdzie `-p` to port

**Wykonanie komendy/skryptu**:
`ssh user@http://192.168.0.1 -p 22 <komenda>`

**Inne opcje**:

* `-p` - port
* `-E` - log file
* `-F` - alt config file - jeśli podany to domyślny `~/.ssh/config` będzie zignorowany
* `-i` - identity file - plik z kluczem prywatnym do danego hosta

**Kopiowanie plików:**
`scp <lokalny_plik> <zdalny_cel>`
`scp ~/katalog/plik.txt user@192.168.0.1/path/to/dir`
opcje jak przy ssh

## SSHfs
Montowanie zdalnych katalogów do lokalnego wystemu plików:
`sshfs -oport=4444 user@192.168.0.1: ~/path/to/dir`
Ważny jest `:` po nazwie zdalnego hosta - można po nim podać ścieżkę do zdalnego katalogu

**Odmontowanie**:
`fusermount -u ~/path/to/dir`

## Serwer
Konfiguracja `sshd` znajduje się w `/etc/ssh/sshd_config`:
* `Port` - port, na którym nasłuchuje `sshd`. Warto zmienić na jakiś >1024
* `ClientAliveInterval XX` - co jaki czas serwer ma wysyłać do klienta pakiety podtrzymujące połączenie (w sekundach)
* `ClientAliveCountMax YY` - maksymalny czas podtrzymywania połączenia (krotność interwału)
