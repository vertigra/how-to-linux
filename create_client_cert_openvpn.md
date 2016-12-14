# Как сделать сертификат клиентa OpenVPN

> OC: Debian 8 Jessie

Генерируем сертификаты

```bash
# cd /etc/openvpn/easy-rsa (или туда где лежит easy-rsa)
# source ./vars && ./build-key clientname
(all question - yes)
```

Добавляем привязку клиента к ip (опционально)

```bash
# echo 'ifconfig-push 10.10.100.13 10.10.100.14' > /etc/openvpn/ccd/clientname
```

Архивируем

```bash
# tar -C /etc/openvpn/easy-rsa/keys -cvzf /etc/openvpn/client1.tar.gz {ca.crt,clientname.crt,clientname.key,client.ovpn,ta.key}
```

Файл client.ovpn подразумевает что предварительно был сконфигурирован файл клиент.

Копируем

```bash
$ sftp user@ip
sftp> cd /etc/openvpn
sftp> get client1.tar.gz
sftp> bye
```
