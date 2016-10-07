# Как отключить IPv6 в Debian 8 Jessie
*OC: Debian 8 Jessie*

```bash
# joe /etc/sysctl.d/99-sysctl.conf
(добавляем)
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
(закоментить)
#::1            localhost ip6-localhost ip6-loopback
```
Запрещаем весь траффик на IPv6 интерфейсе. 
```bash
# joe /etc/iptables/rules.v6
(добавляем)
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



