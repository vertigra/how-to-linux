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
  # ping google.com
  ```
* Меняем пароль суперпользователя
  ```bash
  # passwd root
  ```
* Запускаем демон sshd
  ```bash
  # rc-service sshd start
  ```
* Смотрим внешний ip-адрес сетевой карты 
  ```bash
  livecd ~ # ifconfig
  eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.254.253.226  netmask 255.255.255.0  broadcast 10.254.253.255
        inet6 fe80::44fe:24ff:fe50:913f  prefixlen 64  scopeid 0x20<link>
        ether 46:fe:24:50:91:3f  txqueuelen 1000  (Ethernet)
        RX packets 8104  bytes 1056415 (1.0 MiB)
        RX errors 0  dropped 19  overruns 0  frame 0
        TX packets 972  bytes 883977 (863.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
   ```