# Как управлять iptables
*OC: Debian 8 Jessie*

### Сбросить правила iptables (IPv4 или IPv6)
```bash
# iptables -F && iptables -X
# ip6tables -F && ip6tables -X
```
### Посмотреть текущую конифигруацию iptables (IPv4 или IPv6)
```bash
# iptables -L -nv
# ip6tables -L -nv
```
### Применить новые правила (IPv4 или IPv6)
```bash
# iptables-restore < /etc/iptables/rules.v4
# ip6tables-restore < /etc/iptables/rules.v6
```
