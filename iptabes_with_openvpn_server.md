# Как настроить iptables на сервере OpenVPN
> OC: Debian 8 Jessie

Все хорошо описано [здесь](https://www.linode.com/docs/networking/vpn/set-up-a-hardened-openvpn-server)

Сначала сбросить все существующие правила `# iptables -F && sudo iptables -X`. 

Копируем правила (предварительно скопировав текущую конфигурацию) IPv4.
```bash
# cd /etc/iptables
# mv rules.v4 rules.v4.old
# joe rules.v4
(copy this)

*filter

# Allow all loopback (lo) traffic and reject anything
# to localhost that does not originate from lo.
-A INPUT -i lo -j ACCEPT
-A INPUT ! -i lo -s 127.0.0.0/8 -j REJECT
-A OUTPUT -o lo -j ACCEPT

# Allow ping and ICMP error returns.
-A INPUT -p icmp -m state --state NEW --icmp-type 8 -j ACCEPT
-A INPUT -p icmp -m state --state ESTABLISHED,RELATED -j ACCEPT
-A OUTPUT -p icmp -j ACCEPT

# Allow SSH.
-A INPUT -i eth0 -p tcp -m state --state NEW,ESTABLISHED --dport 22 -j ACCEPT
-A OUTPUT -o eth0 -p tcp -m state --state ESTABLISHED --sport 22 -j ACCEPT

# Allow UDP traffic on port 1194.
-A INPUT -i eth0 -p udp -m state --state NEW,ESTABLISHED --dport 1194 -j ACCEPT
-A OUTPUT -o eth0 -p udp -m state --state ESTABLISHED --sport 1194 -j ACCEPT

# Allow DNS resolution and limited HTTP/S on eth0.
# Necessary for updating the server and keeping time.
-A INPUT -i eth0 -p udp -m state --state ESTABLISHED --sport 53 -j ACCEPT
-A OUTPUT -o eth0 -p udp -m state --state NEW,ESTABLISHED --dport 53 -j ACCEPT
-A INPUT -i eth0 -p tcp -m state --state ESTABLISHED --sport 80 -j ACCEPT
-A INPUT -i eth0 -p tcp -m state --state ESTABLISHED --sport 443 -j ACCEPT
-A OUTPUT -o eth0 -p tcp -m state --state NEW,ESTABLISHED --dport 80 -j ACCEPT
-A OUTPUT -o eth0 -p tcp -m state --state NEW,ESTABLISHED --dport 443 -j ACCEPT

# Allow traffic on the TUN interface.
-A INPUT -i tun0 -j ACCEPT
-A OUTPUT -o tun0 -j ACCEPT

# Log any packets which don't fit the rules above...
# (optional but useful)
-A INPUT -m limit --limit 3/min -j LOG --log-prefix "iptables_INPUT_denied: " --log-level 4
-A FORWARD -m limit --limit 3/min -j LOG --log-prefix "iptables_FORWARD_denied: " --log-level 4
-A OUTPUT -m limit --limit 3/min -j LOG --log-prefix "iptables_OUTPUT_denied: " --log-level 4

# then reject them.
-A INPUT -j REJECT
-A FORWARD -j REJECT
-A OUTPUT -j REJECT

COMMIT
```

**После `COMMIT` должна быть `newline` (переходим в конец файла и жмем `Enter`)**

Потом [выключаем IPv6](https://linux.nesterof.com/disable_ipv6.html)