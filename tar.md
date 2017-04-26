# Как создать tar.gz

> OC: Debian 8 Jessie

```bash
tar -zcvf file.tar.gz /full/path — создать .tar.gz (архив)
tar -jcvf file.tar.bz2 /full/path — создать .tar.bz2 (архив)

tar -cvf file.tar /full/path — создать .tar
```

Синтаксис этих примеров:

tar [-ключи] [название архива] [путь, что запаковать]

<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-98112747-1', 'auto');
  ga('send', 'pageview');

</script>