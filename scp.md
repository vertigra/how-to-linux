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

Следует знать что:
```txt
By default scp uses the Triple-DES cipher to encrypt the data being sent. Using the Blowfish cipher has been shown to increase speed. This can be done by using option -c blowfish in the command line.
```
На Debian опция `-c blowfish` дает ошибку:
```bash
no matching cipher found: client blowfish-cbc server aes128-ctr,aes192-ctr,aes256-ctr,aes128-gcm@openssh.com,aes256-gcm@openssh.com,chacha20-poly1305@openssh.com
```
Соответственно следует использовать один из предлженых режимов шифрования: 
```bash
$ scp -c aes256-gcm@openssh.com -P 2024 test_file user@ip:/home/user
```

