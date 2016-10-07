# Первоначальная настройка Debain-8.0-x86_64-minimal) после исталяции
*OC: Debian 8 Jessie*

Обновляемся
```bash
# apt-get update && apt-get upgrade
```

Устанавливаем нужный софт (зачем нужен [dialog](https://linux.nesterof.com/error_dailog.html) 
```bash
# apt-get install dialog && apt-get install joe && apt-get install mc && apt-get install iptables-persistent
(on all question - yes)
```

По пунктам: 
* dialog зачем нужен [здесь](https://linux.nesterof.com/error_dailog.html) 
* joe - текстовый редактор
* mc - менеджер файлов
* iptables-persistent о нем [здесь](https://linux.nesterof.com/setting-iptables-after-install.html)

Копируем правила фаервола IPv4:
```bash
# joe /etc/iptables/rules.v4
(copy this)
*filter

# Allow all loopback (lo0) traffic and reject traffic
# to localhost that does not originate from lo0.
-A INPUT -i lo -j ACCEPT
-A INPUT ! -i lo -s 127.0.0.0/8 -j REJECT

# Allow ping.
-A INPUT -p icmp -m state --state NEW --icmp-type 8 -j ACCEPT

# Allow SSH connections.
-A INPUT -p tcp --dport 22 -m state --state NEW -j ACCEPT

# Allow inbound traffic from established connections.
# This includes ICMP error returns.
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Log what was incoming but denied (optional but useful).
-A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables_INPUT_denied: " --log-level 7

# Reject all other inbound.
-A INPUT -j REJECT

# Log any traffic that was sent to you
# for forwarding (optional but useful).
-A FORWARD -m limit --limit 5/min -j LOG --log-prefix "iptables_FORWARD_denied: " --log-level 7

# Reject all traffic forwarding.
-A FORWARD -j REJECT

COMMIT
```