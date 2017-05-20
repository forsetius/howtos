# Nginx

## Bezpieczeństwo

1. Wyłączamy zbyt wiele mówiące nagłówki
```
http {
    server_tokens off;
    ...
}
```

2. W `php.ini` ustawiamy `expose_php = Off`

3. Zamykamy dostęp do szczególnie wrażliwych obszarów spoza zaufanych IP
```
server {
    location /wp-admin/ {
        allow 192.168.1.1/24;
        allow 10.0.0.1/24;
        deny  all;
    }
}
```