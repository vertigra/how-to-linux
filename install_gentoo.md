# Как установить Gentoo с minimal-cd
---
***NOTE*** 

*Для сборки ядра и модулей необходим раздел root не меньше 12 Гб *

---
***NOTE***

*Инсталяция производилась на виртуальную машину (Citrix Xen Server) поэтому имя жесткого диска и его разделов (/dev/xvda, /dev/xvda1 /dev/xvda2, /dev/xvda3) на реальном железе будет отличаться (sda, sda1, sda2, sda3)*

---
***NOTE***

*Конфигурация виртуальной машины на которую производилась инсталяция: процессор (Vendor: AuthenticAMD, Model: AMD Phenom(tm) II X4 945 Processor, Speed: 3013 MHz) - Number of vCPUs - 2, Topology - 2 sockets with 1 core per socket, vCPU priority - Normal; память 512 Mb; жесткий диск -13Гб, position 0; сеть DHCP. Время установки 6-7 часов.*

---
***NOTE***

*Устанавливалось с носителя: [gentoo-install-amd64-minimal-20161103.iso](http://ftp.halifax.rwth-aachen.de/gentoo/releases/amd64/autobuilds/20161103/install-amd64-minimal-20161103.iso). Версия установки - чистая консоль без поддержки X-сервера*

По мотивам [этой](http://www.oldnix.org/install-gentoo-linux/) и [этой](http://www.r-notes.ru/administrirovanie/gentoo-linux/116-gentoo-tipovaya-ustanovka.html) статьи с [продолжением](http://www.r-notes.ru/administrirovanie/gentoo-linux/117-gentoo-tipovaya-ustanovka-chast-2.html).

### Подготовка (консоль сервера) ###

* Скачиваем minimal installation cd c сайта [gentoo.org](https://gentoo.org/downloads/) и пишем его на флэшку или диск
* После загрузки с диска проверяем сеть (у меня DHCP поэтому дополнтельно настраивать ничего не нужно)
  ```bash
  livecd ~ # ping google.com
  ```
  
* Меняем пароль суперпользователя
  ```bash
  livecd ~ # passwd root
  ```
  
* Запускаем демон sshd
  ```bash
  livecd ~ # rc-service sshd start
  ```
  
* Смотрим внешний ip-адрес сетевой карты и подключаемся к нему по ssh 
  ```bash
  livecd ~ # ifconfig
  eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.226  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::44fe:24ff:fe50:913f  prefixlen 64  scopeid 0x20<link>
        ether 46:fe:24:50:91:3f  txqueuelen 1000  (Ethernet)
        RX packets 8104  bytes 1056415 (1.0 MiB)
        RX errors 0  dropped 19  overruns 0  frame 0
        TX packets 972  bytes 883977 (863.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
   ```
   
### Разметка диска (ssh)

* Так как машина тестовая, то будет использоваться упрощенная разметка диска: 
  ```
  /boot - 100 Mb 
  /swap - 1 Gb
  /root - 11.9 Gb
  ```
  Хорошо написано про разметку с помощью fdisk [здесь](http://www.oldnix.org/fdisk-linux/).
* Проверяем существующую разметку - команда `p`
  ```bash
  livecd ~ # fdisk /dev/xvda

  Welcome to fdisk (util-linux 2.26.2).
  Changes will remain in memory only, until you decide to write them.
  Be careful before using the write command.

  Device does not contain a recognized partition table.
  Created a new DOS disklabel with disk identifier 0x0ff8c11f.
  ----
  Command (m for help): p
  Disk /dev/xvda: 13 GiB, 13958643712 bytes, 27262976 sectors
  Units: sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disklabel type: dos
  Disk identifier: 0xad15584a
  ```

* Размечаем раздел /boot (100 мегабайт) - команда `n`
  ```bash
  Command (m for help): n
  Partition type
     p   primary (0 primary, 0 extended, 4 free)
     e   extended (container for logical partitions)
  Select (default p):

  Using default response p.
  Partition number (1-4, default 1):
  First sector (2048-27262976, default 2048):
  Last sector, +sectors or +size{K,M,G,T,P} (2048-27262976, default 27262976): +100M

  Created a new partition 1 of type 'Linux' and of size 100 MiB.
  ```
  На все запросы fdisk оставляем значния по умолчанию, кроме запроса last sector, там пишем +100M.
  
* Размечаем раздел под swap
  ```bash
  Command (m for help): n
  Partition type
     p   primary (1 primary, 0 extended, 3 free)
     e   extended (container for logical partitions)
  Select (default p):

  Using default response p.
  Partition number (2-4, default 2):
  First sector (206848-27262976, default 206848):
  Last sector, +sectors or +size{K,M,G,T,P} (206848-27262976, default 27262976): +1G

  Created a new partition 2 of type 'Linux' and of size 1 GiB.
  ```
  Так же оставляем все значения по умолчанию, в запросе last sector пишем +1G.

* Размечаем раздел под root
  ```bash
  Command (m for help): n
  Partition type
     p   primary (2 primary, 0 extended, 2 free)
     e   extended (container for logical partitions)
  Select (default p):

  Using default response p.
  Partition number (3,4, default 3):
  First sector (2304000-27262976, default 2304000):
  Last sector, +sectors or +size{K,M,G,T,P} (2304000-27262976, default 27262976):

  Created a new partition 3 of type 'Linux' and of size 11.9 GiB.
  ```
  Здесь все запросы включая last sector - по умолчанию.

* Меняем тип раздела для swap - команда `t`
  ```bash
  Command (m for help): t
  Partition number (1-3, default 3): 2
  Partition type (type L to list all types): 82

  Changed type of partition 'Linux' to 'Linux swap / Solaris'
  ```
  Для раздела 2 (Partition number) указываем тип раздела (Partition type) 82.
  
* Проверяем что получилось - команда `p`
  ```bash
  Command (m for help): p
  Disk /dev/xvda: 13 GiB, 13958643712 bytes, 27262976 sectors
  Units: sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disklabel type: dos
  Disk identifier: 0xad15584a

  Device     Boot   Start      End  Sectors  Size Id Type
  /dev/xvda1         2048   206847   204800  100M 83 Linux
  /dev/xvda2       206848  2303999  2097152    1G 82 Linux swap / Solaris
  /dev/xvda3      2304000 27262975 24958976 11.9G 83 Linux
  ```
* Если все правильно, сохраняем - команда `w`
  ```bash
  Command (m for help): w
  The partition table has been altered.
  Calling ioctl() to re-read partition table.
  Syncing disks.
  ```

### Форматирование диска (ssh)

* Форматируем файловые системы. Для раздела /boot - ext2. Для раздела /root - ext4. Для раздела подкачки - swap.
  ```bash
  livecd ~ # mkfs.ext2 -L boot /dev/xvda1
  livecd ~ # mkfs.ext4 -L root /dev/xvda3
  livecd ~ # mkswap -L swap /dev/xvda2
  ```
* Подключаем своп
  ```bash
  livecd ~ # swapon /dev/xvda2
  ```

### Монтирование разделов (ssh)

* Монтируем созданые разделы. Точка монтирования `/mnt/gentoo`
  ```bash
  livecd ~ # mount /dev/xvda3 /mnt/gentoo
  livecd ~ # mkdir /mnt/gentoo/boot
  livecd ~ # mount /dev/xvda1 /mnt/gentoo/boot
  ```
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  