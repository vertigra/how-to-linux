# Как установить Java 8 \(JDK/JRE 8u101\) с использованием PPA

> OC: Debian 8 Jessie

Взято [отсюда](http://tecadmin.net/install-java-8-on-debian/)

```bash
#  joe /etc/apt/sources.list.d/java-8-debian.list
(add this)
deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main
deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main

# apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886
# apt-get update
# apt-get install oracle-java8-installer
(yes on license terms)

# java -version
java version "1.8.0_101"
Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)

# apt-get install oracle-java8-set-default
```


<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-98112747-1', 'auto');
  ga('send', 'pageview');

</script>
