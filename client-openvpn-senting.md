# Как установить клиент OpenVPN на Debian 8 Jessie

> OC: Debian 8 Jessie

```bash
# apt-get install openvpn
(Копируем сертификаты в /etc/openvpn)
```

Переименовываем конфигурацию для windows клиентов (.ovpn) на конфигурацию для linux клиентов (.conf).

```bash
# mv client.ovpn client.conf
# joe client.conf
```

Меняем название сертификатов, раскоменчиваем строки логов и пользователя nobody от которого будет запускаться демон

```bash
log-append /var/log/openvpn/client.log
status /var/log/openvpn/status.log

user nobody
group nogroup
```

Создаем дирректорию для логов

```bash
# mkdir /var/log/openvpn
# systemctl enable openvpn
# service openvpn restart
```