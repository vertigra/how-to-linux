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

<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-98112747-1', 'auto');
  ga('send', 'pageview');

</script>
