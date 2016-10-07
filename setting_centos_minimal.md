# Первоначальная настройка Debain-8.0-x86_64-minimal) после исталяции
*OC: Debian 8 Jessie*

Обновляемся
```bash
# apt-get update && apt-get upgrade
```

Устанавливаем нужный софт
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

Копируем правила фаервола IPv6 (запрещаем весь трафик по IPv6):
```bash
# joe /etc/iptables/rules.v6
(copy this)

*filter

-A INPUT -j REJECT
-A FORWARD -j REJECT
-A OUTPUT -j REJECT
COMMIT
```

Применяем правила:
```bash
# iptables-restore < /etc/iptables/rules.v4 && ip6tables-restore < /etc/iptables/rules.v6
```

Добавляем нового пользователя командой `adduser username` (указываем пароль и указываем имя и номера телефонов, кроме пароля все необязательно).

Перевешиваем SSH на другой порт и запрещаем логиниться под пользвателем root.
```bash
# joe /etc/ssh/sshd_config

PermitRootLogin yes change on PermitRootLogin no

Port 22 change on Port 2500 (any)
```

**Меняем в правилах фаервола порт 22 на тот что указали в sshd_config**
```bash
# joe /etc/iptables/rules.v4

# Allow SSH connections.
-A INPUT -p tcp --dport 22 -m state --state NEW -j ACCEPT
(change)
-A INPUT -p tcp --dport 2500 -m state --state NEW -j ACCEPT
```

Перезагружаемся и проверяем - должен измениться порт подключения ssh, логин от пользователя root должен выдавать ошибку `Permission denied, please try again.` (залогиниться можно только от вновь созданого пользователя и повысить привелигии до `root` командой `su`). Так же можно проверить статус iptables `




