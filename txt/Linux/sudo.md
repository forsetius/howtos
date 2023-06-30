# Sudo

## Użytkownik

Domyślnie `sudo` przełącza na użytkownika **root**. Aby przełączyć się na innego użytkownika należy użyć opcji `-u <username>`:

```sh
sudo -u www mc /var/www/html
```

## Katalog użytkownika

W Ubuntu proces uruchomiony przez `sudo` domyślnie ustawia zmienną `HOME` na katalog ***sudoera***. Aby ustawić go na katalog ***roota*** (czy innego użytkownika, na którego się przełączamy) trzeba użyć opcji `-H`:

```sh
sudo -H mc
```

Jest to przydatne przy Midnight Commanderze, który swoją konfigurację czyta z `$HOME/.config/mc`. Bez `-H` będzie czytał ustawienia, w tym skórkę, ***sudoera*** nie ***roota***.
