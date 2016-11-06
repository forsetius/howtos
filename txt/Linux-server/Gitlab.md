# Gitlab

## Nginx obok Apache - 2 IP

Para IP+port może być obsługiwana tylko przez jeden serwer HTTP. Dlatego mając 2 IP można do każdego przypisać inny httpd.

1. Zamawiamy nowy IP
2. Nowy IP trzeba podpiąć do interfejsu sieciowego jako alias:
  1. Sprawdzamy nazwę interfejsu głównego:
    `ifconfig -as`
    nie będzie to `lo` ani `wifi`. Załóżmy, że to `eth0`.
  2. W `/etc/network/interfaces` lub `/etc/network/interfaces.d/*.cfg`:
    ```
    auto eth0:1
    iface eth0:1 inet static
    name Ethernet alias LAN card
    address 192.168.1.7
    netmask 255.255.255.0
    broadcast 192.168.1.255
    network 192.168.1.0
    ```

    Jeśli dostajemy tylko 1 IP od ISP to raczej dane będą wyglądały tak:
    ```
    address NOWE.IP.OD.ISP
    netmask 255.255.255.255
    broadcast NOWE.IP.OD.ISP
    ```
  3. Podnosimy interfejs:
    `ifup eth0:1`
  4. Sprawdzamy NOWE.IP.OD.ISP:
    `ping -c 4 NOWE.IP.OD.ISP`
3. W panelu u ISP w konfiguracji domeny, na strefie DNS przypisujemy IP do domeny i subdomen
4. W konfiguracji Apache ustawiamy nasłuch na NOWE.IP.OD.ISP.
  1. W `/etc/apache2/ports.conf`:
    `Listen NOWE.IP.OD.ISP:80`
  2. Ew. to samo w konfiguracji SSL.
  3. Restart Apache:
    `sudo service apache2 restart`
5. W konfiguracji Gitlaba ustawiamy nasłuch Nginx na STARE.IP.OD.ISP.
  1. W pliku konfiguracyjnym `/etc/gitlab/gitlab.rb`:
    `nginx['listen_addresses'] = ['STARE.IP.OD.ISP']`
  2. Restart Gitlaba:
    `sudo gitlab-ctl reconfigure`
