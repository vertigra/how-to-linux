# Как поднять клиент NFS
*OC: Debian 8 Jessie*

```bash
# apt-get install nfs-client
# joe /etc/idmapd.conf

(uncoment this)
Domain = test.localdomain

# tests mount
# mount -t nfs 192.168.1.1:/mnt/nfs /mnt
```