# Как поднять сервер NFS в CentOS 6.8
*OC: CentOS 6.8(Final)*

```bash
# yum install nfs-utils nfs-utils-lib
# service rpcbind start
# service nfs start
Starting NFS services:                                     [  OK  ]
Starting NFS mountd:                                       [  OK  ]
Starting NFS daemon:                                       [  OK  ]
Starting RPC idmapd:                                       [  OK  ]
```
Редактируем конфигурационный файл NFS сервера `joe /etc/exports`

```bash
/mnt/share 192.168.1.0/24(rw)
, где:
/mnt/share - путь к папке, для которой раздается доступ;
192.168.1.0/24 - подсеть для который будет доступен расшаренный ресурс;



```