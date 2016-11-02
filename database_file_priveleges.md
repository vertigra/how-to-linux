# Какие права назначить на дирректорию и файл базы данных Firebird
*OC: Debian 8 Jessie (x64)*

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
  
 
 