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