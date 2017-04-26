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

<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-98112747-1', 'auto');
  ga('send', 'pageview');

</script>