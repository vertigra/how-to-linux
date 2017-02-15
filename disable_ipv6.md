# Как отключить IPv6 в Debian 8 Jessie

> OC: Debian 8 Jessie*

```bash
# joe /etc/sysctl.d/99-sysctl.conf
(copy this)
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
net.ipv6.conf.eth0.disable_ipv6 = 1 (изменить на свой внешний интерфейс)
```

Применяем изменения

```bash
# sysctl -p
```

```bash
# joe /etc/hosts
(commented)
#::1            localhost ip6-localhost ip6-loopback
```

Запрещаем весь траффик на IPv6 интерфейсе. 

```bash
# joe /etc/iptables/rules.v6
(copy this)
*filter

-A INPUT -j REJECT
-A FORWARD -j REJECT
-A OUTPUT -j REJECT
COMMIT
```

Применяем правила и проверяем.

```bash
# ip6tables-restore < /etc/iptables/rules.v6
# ip6tables -vL for v6
```

Так же надо знать что:

**APT attempts to resolve mirror domains to IPv6 as a result of apt-get update. If you choose to entirely disable and deny IPv6, this will slow down the update process for Debian and Ubuntu because APT waits for each resolution to time out before moving on.**

```bash
# joe /etc/gai.conf.
(uncomment)
precedence ::ffff:0:0/96 100
```

***UPD** [Здесь](http://vacadem.ru/blog/linux-unix-and-other/disable-ipv6-in-debian.html) пишут что надо еще так: 

> В файле /etc/netconfig закомментировать строки

```bash
#udp6       tpi_clts      v     inet6    udp     -       -
#tcp6       tpi_cots_ord  v     inet6    tcp     -       -
```

> В файле /etc/hosts закомментировать **все строки**, относящиеся к IPv6

```bash
# The following lines are desirable for IPv6 capable hosts
#::1     ip6-localhost ip6-loopback
#fe00::0 ip6-localnet
#ff00::0 ip6-mcastprefix
#ff02::1 ip6-allnodes
#ff02::2 ip6-allrouters
```

> В /etc/ssh/ssh_config

```bash
AddressFamily inet
```

В /etc/ssh/sshd_config раскомментировать строку ListenAddress для ipv4

```bash
#ListenAddress ::
ListenAddress 0.0.0.0
```

> В /etc/exim4/update-exim4.conf

```bash
disable_ipv6 = true
```

> Перегрузить и убедиться что интерфейс не имеет адреса IPv6