# Как поднять клиент NFS
*OC: Debian 8 Jessie*

```bash
# apt-get install nfs-client
# joe /etc/idmapd.conf

(uncoment this)
Domain = test.localdomain
```