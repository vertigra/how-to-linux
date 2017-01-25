# Как правильно выключить Citrix Xen Server
> OC: XEN 6.5

Что бы выключить:

```bash
xe host-disable host
xe vm-shutdown --multiple
xe host-shutdown
```

Что бы перегрузить

```bash
> xe host-disable host
> xe vm-shutdown
> xe host-reboot
```

Простейший скрипт (с заходом по ключу) `xen_host_ip` - ip сервера

```bash
ssh root@xen_host_ip "xe host-disable ; xe vm-shutdown --multiple ; xe host-shutdown"
```

