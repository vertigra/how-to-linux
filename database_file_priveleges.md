# Какие права назначить на дирректорию и файл базы данных Firebird

[Копия](https://nesterof.com/2017/04/21/install_firebird_on_debian/)

> OC: Debian 8 Jessie (x64)

Основано на [этой](http://www.firebirdfaq.org/faq102/) статье.

* Добавить себя в группу firebird
  ```bash
  # addgroup username firebird
  ```

* Права на дирректорию БД 770, пользователь и группа firebird
  ```bash
  # chown -R firebird:firebird /databasedir (R - recursive)
  # chmod 770 /databasedir
  ```

* Права на файл базы данных 660, пользователь и группа firebird
  ```bash
  # chown firebird:firebird datbasefilename.fdb
  # chmod 660 datbasefilename.fdb
  ```

Пример использовния утилиты gbak (бэкап базы данных)

```bash
$ gbak -b -g -v -user SYSDBA -password masterkey datbasefilename.fdb testbackup.fbk
```