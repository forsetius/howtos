# Samba

## Instalacja
```sh
sudo apt install samba winbind system-config-samba
```

## Konfiguracja
Plik `/etc/samba/smb.conf`:
```ini
[global]
    workgroup = glitnir
    server string = %h file server
    wins support = yes
    wins server = 192.168.8.10
    server role = standalone server
;    os version = 255                  # dla głównego serwera
    os version = 33                   # dla innych serwerów
```

Dalej konfigurujemy za pomocą `system-config-samba`.
Ze względu na błąd w pakiecie należy po jego instalacji wykonać:
```sh
sudo su
touch /etc/libuser.conf
chmod 777 /etc/libuser.conf
system-config-samba
```
