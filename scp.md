# Как забрать/положить файлы через SCP
*OC: Debian 8 Jessie*

Положить файл с локальной дирректории в удаленную
```bash
scp file.tar.gz user@ip:/home/user
```

Положить файл с локальной дирректории в удаленную через нестандартный порт SSH(2500)
```bash
scp -P 2500 la-serv.tar.gz user@ip:/home/user
```

Следует знать что:
```
By default scp uses the Triple-DES cipher to encrypt the data being sent. Using the Blowfish cipher has been shown to increase speed. This can be done by using option -c blowfish in the command line.
```
