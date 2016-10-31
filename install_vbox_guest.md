# Как установить VboxGuestAddition
*OC: Debian 8 Jessie*

```bash
# apt-get install build-essential module-assistant
# m-a prepare
```
Монтируем образ дополнений из меню VirtualBox: Устройства -> Подключить образ диска дополнений гостевой OC...
```bash
# mount /dev/cdrom /mnt
# sh /mnt/VBoxLinuxAdditions.run
```
При появлении ошибки ```Error: unable to find the sources of your current Linux kernel...``` следует выполнить 
```bash
# apt-cache search linux-headers-$(uname -r)
# apt-get install linux-headers-$(uname -r)
```