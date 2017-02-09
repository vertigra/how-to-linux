# Как добавить или удалить службу в Debian

> OC: Debian 8 Jessie

Что бы добавить службу foo (должен присутсвовать скрипт в `/etc/init.d`):

```bash
# update-rc.d foo defaults
```

Что бы удалить службу foo:

``` bash
# update-rc.d foo remove
```

Дополнительные параметры:

```bash
usage: update-rc.d [-n] [-f] <basename> remove
       update-rc.d [-n] <basename> disable|enable [S|2|3|4|5]
                -n: not really
                -f: force

```

Взято [тут](http://www.infodrom.org/Debian/doc/maint/Maintenance-runlevel.html)
