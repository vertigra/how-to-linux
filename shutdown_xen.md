# Как правильно выключить Citrix Xen Server
> OC: XEN 6.5

[Основная статья](https://nesterof.com/2017/05/01/how-shutdown-citrix-xen-server/)

Что бы выключить:

```bash
# xe host-disable host
# xe vm-shutdown --multiple
# xe host-shutdown
```

Что бы перегрузить

```bash
# xe host-disable host
# xe vm-shutdown --multiple
# xe host-reboot
```

Простейший скрипт (с заходом по ключу) `xen_host_ip` - ip сервера

```bash
ssh root@xen_host_ip "xe host-disable ; xe vm-shutdown --multiple ; xe host-shutdown"
```

Статьи по теме:

1. [blog.henrikandersen.se](http://blog.henrikandersen.se/2011/05/02/starting-and-stopping-xenserver-with-the-xe-command/)
2. [superuser.com](http://superuser.com/questions/1062931/shut-down-all-vms-on-xenserver-6-5-0)


