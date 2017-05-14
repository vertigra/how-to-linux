# Как управлять iptables

[Основная статья](https://nesterof.com/2017/05/15/how-to-manage-iptables/)

> OC: Debian 8 Jessie

## Сбросить правила iptables (IPv4 или IPv6)

```bash
# iptables -F && iptables -X
# ip6tables -F && ip6tables -X
```

## Посмотреть текущий статус iptables (IPv4 или IPv6)

```bash
# iptables -L -nv
# ip6tables -L -nv
```

## Применить новые правила (IPv4 или IPv6)

```bash
# iptables-restore < /etc/iptables/rules.v4
# ip6tables-restore < /etc/iptables/rules.v6
```

## Выключить iptables (IPv4)

```bash
# iptables-save > $HOME/firewall.txt
# iptables -X
# iptables -t nat -F
# iptables -t nat -X
# iptables -t mangle -F
# iptables -t mangle -X
# iptables -P INPUT ACCEPT
# iptables -P FORWARD ACCEPT
# iptables -P OUTPUT ACCEPT
```

## Выключить ip6tables (IPv6)

```bash
# ip6tables-save > $HOME/firewall-6.txt
# ip6tables -X
# ip6tables -t mangle -F
# ip6tables -t mangle -X
# ip6tables -P INPUT ACCEPT
# ip6tables -P FORWARD ACCEPT
# ip6tables -P OUTPUT ACCEPT
```