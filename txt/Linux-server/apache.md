# Apache2
#### Serwer WWW

## Użycie
- Włącz, wyłącz, zatrzymaj i włącz, status:
  `sudo service apache2 start | stop | restart | status`
- Sprawdź konfigurację:
  `sudo apache2ctl configtest`

## Konfiguracja
Konfiguracja znajduje się w dystrybucjach Debianowych w `/etc/apache2`. DocumentRoot - główny katalog, z którego serwowane są pliki to domyślnie `/var/www/html`.

Moją praktyką jest zakładanie katalogów z poszczególnymi serwowanymi projektami w katalogu domowym i łączenie ich symlinkami z `/var/www/html`.

### Moduły
#### DirectoryIndex
Nazwy plików interpretowane jako index
Dopasowywanie w kolejności od lewej do prawej
```apache
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

#### LanguagePriority
Kolejność dopasowania języków
<IfModule mod_negotiation.c>
    LanguagePriority pl en ca cs da de el eo es et fr he hr it ja ko ltz nl nn no
</IfModule>

#### UserDir
```
<IfModule mod_userdir.c>
    UserDir www
    UserDir disabled root

    <Directory /home/*/www>
        AllowOverride FileInfo AuthConfig Limit Indexes
        Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
        <Limit GET POST OPTIONS>
            Require all granted
        </Limit>
        <LimitExcept GET POST OPTIONS>
            Require all denied
        </LimitExcept>
    </Directory>
</IfModule>
```

Formy zapisu UserDir:

| Dla http://example.com/~forseti | Katalog użytkownika |
| --- | --- |
| UserDir public_html | /home/forseti/public_html |
| UserDir /var/html | /var/html/forseti |
| UserDir /var/www/*/docs | /var/www/forseti/docs |
| UserDir http://example.org/users/*/ | http://example.org/users/forseti |

UserDir może być włączany i wyłączany dla określonych użytkowników:

|   |   |
| --- | --- |
| Włączony dla wszystkich, Wyłączony dla root | UserDir disabled root |
| Wyłączony dla wszystkich, Włączony dla forseti | UserDir disabled  <br>UserDir enabled forseti |

TODO: rewrite z localhost/~forseti fo forseti.localhost

### Virtual hosty

```
Listen 8080
<VirtualHost *:8080>
    ServerName paragraf.local

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/Paragraf/web

    <Directory /var/www/html/Paragraf/web>
        Options Indexes FollowSymlinks Multiviews
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
    php_flag log_errors on
    php_flag display_errors on
    php_value error_reporting 2147483647
    php_value error_log /var/www/domains/epgen/php.error.log
</VirtualHost>
```

### SELinux

https://fedoraproject.org/wiki/SELinux/apache
```
sudo setsebool -P httpd_enable_homedirs 1
sudo chcon -R -t httpd_sys_content_t /home/forseti/www
```
