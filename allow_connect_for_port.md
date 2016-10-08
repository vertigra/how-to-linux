# Как разрешить в iptables исходящий трафик для определеннного порта
*OC: Debian 8 Jessie*

Если конфигурацией фаервола запрещен исходящий траффик
```bash
-A INPUT -j REJECT
-A FORWARD -j REJECT
-A OUTPUT -j REJECT
```

как [здесь](https://linux.nesterof.com/iptabes_with_openvpn_server.html). То разрешить **исходящий** траффие можно так:
```bash
# Allow TCP output traffic on port 2500.
-A INPUT -i eth0 -p tcp -m state --state ESTABLISHED --sport 2500 -j ACCEPT
-A OUTPUT -o eth0 -p tcp -m state --state NEW,ESTABLISHED --dport 2500 -j ACCEPT
```

Эта цепочка правил разрешит исходящие соединения (и ответный входящий траффик) на порту 2500. Например исходящие SSH соединения на порт 2500. 

При этом, если на порту 2500 висит какая-то сетевая служба (`LISTEN`) то она по-прежнему останется недоступной(закрытой) для указаного сетевого интерфейса. 

А такая цепочка наоборот откроет входящие соединения на порт 10206.
```bash
# Allow UDP input traffic on port 10206.
-A INPUT -i venet0 -p udp -m state --state NEW,ESTABLISHED --dport 10206 -j  ACCEPT
-A OUTPUT -o venet0 -p udp -m state --state ESTABLISHED --sport 10206 -j ACCEPT
```