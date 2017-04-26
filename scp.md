# Как забрать/положить файлы через SCP
*OC: Debian 8 Jessie*

Положить файл с локальной дирректории в удаленную
```bash
$ scp file.tar.gz user@ip:/home/user
```

Положить файл с локальной дирректории в удаленную через нестандартный порт SSH(2500)
```bash
$ scp -P 2500 la-serv.tar.gz user@ip:/home/user
```

**Следует знать что:**

By default scp uses the Triple-DES cipher to encrypt the data being sent. Using the Blowfish cipher has been shown to increase speed. This can be done by using option -c blowfish in the command line.

На Debian опция `-c blowfish` дает ошибку:
```bash
no matching cipher found: client blowfish-cbc server aes128-ctr,aes192-ctr,aes256-ctr,aes128-gcm@openssh.com,aes256-gcm@openssh.com,chacha20-poly1305@openssh.com
```
Соответственно следует использовать один из предлженых режимов шифрования: 
```bash
$ scp -c aes256-gcm@openssh.com test_file user@ip:/home/user
```

<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-98112747-1', 'auto');
  ga('send', 'pageview');

</script>