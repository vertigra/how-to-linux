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