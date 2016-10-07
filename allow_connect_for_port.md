# Как разрешить в iptables исходящий трафик для определеннного порта
*OC: Debian 8 Jessie*

Если конфигурацией фаервола запрещен исходящий траффик
```bash
-A INPUT -j REJECT
-A FORWARD -j REJECT
-A OUTPUT -j REJECT
```

как например [здесь](https://linux.nesterof.com/iptabes_with_openvpn_server.html), то для того, что бы разрешить **исходящий** траффик определенному приложению на заданном порту (и ответный входящий траффик), необходимо добавить в цепочку следующие правила
```bash
-A INPUT -i eth0 -p tcp -m state --state ESTABLISHED --sport 2500 -j ACCEPT
-A OUTPUT -o eth0 -p tcp -m state --state NEW,ESTABLISHED --dport 2500 -j ACCEPT
```
где,

* `INPUT` — для входящих пакетов адресованных непосредственно локальному процессу (клиенту или серверу).
* `OUTPUT` — для пакетов генерируемых локальными процессами.  
* `eth0` - сетевой интерфейс
* `-p tcp` — транспортный протокол
* `--state ESTABLISHED` — пакет является частью уже существующего сеанса. 
* `--state NEW` — пакет открывает новый сеанс

Эта цепочка правил разрешит исходящие ssh соединения (и ответный входящий траффик) на порту 2500 (`$ ssh -p 2500 user@ip`). При этом, если на порту 2500 висит какая-то сетевая служба (`LISTEN`) то она по-прежнему останется недоступной(закрытой) для указаного сетевого интерфейса. 

А такая цепочка наоборот откроет входящие соединения на порт 10206.
```bash
# Allow UDP traffic on port 10206.
-A INPUT -i venet0 -p udp -m state --state NEW,ESTABLISHED --dport 10206 -j  ACCEPT
-A OUTPUT -o venet0 -p udp -m state --state ESTABLISHED --sport 10206 -j ACCEPT
```