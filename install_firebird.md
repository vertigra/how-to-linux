# Как установить Firebird 1.5.6 \(x32\) на Debian 8 \(x64\)

[Основная статья](https://nesterof.com/2017/04/21/install_firebird_on_debian/)

> OC: Debian 8 Jessie

Основано на [этой](http://blog.elve.name/?p=608) статье, которая немного устарела и не работает.

```bash
# dpkg --add-architecture i386
# apt-get update
# apt-get install libstdc++5:i386 xinetd lib32ncurses5
# cd ~
# wget http://downloads.sourceforge.net/project/firebird/firebird-linux-i386/1.5.6-Release/FirebirdCS-1.5.6.5026-0.i686.tar.gz
# tar xzvf FirebirdCS-1.5.6.5026-0.i686.tar.gz
# cd FirebirdCS-1.5.6.5026-0.i686
# ./install.sh
```

После команды `./install.sh` увидим следующее

```bash
Firebird classic 1.5.6.5026-0.i686 Installation

Press Enter to start installation or ^C to abort
Extracting install data
Please enter new password for SYSDBA user: masterkey
GSEC> Warning - maximum 8 significant bytes of password used
GSEC>
Install completed
```

Утилита GSEC предупреждает - максимальная длина пароля 8 символов \(все остальные будут отброшены\).

Теперь следует прописать в `/etc/ptofile`  путь до бинарных файлов firebird.

```bash
joe /etc/profile 
(add in last line in file)
PATH=$PATH:/opt/firebird/bin
```

И удалить инсталяционные файлы сервера.

```bash
# rm -R ~/FirebirdCS-1.5.6.5026-0.i686
# rm ~/FirebirdCS-1.5.6.5026-0.i686.tar.gz
```

После чего нужно [настроить права дирректории и файла базы данных](https://linux.nesterof.com/database_file_priveleges.html).

