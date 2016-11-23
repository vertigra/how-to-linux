# Как установить Gentoo с minimal-cd
---
***NOTE*** 

*Для сборки ядра и модулей необходим раздел root не меньше 12 Гб *

---
***NOTE***

*Инсталяция производилась на виртуальную машину (Citrix Xen Server) поэтому имя жесткого диска и его разделов (/dev/xvda, /dev/xvda1, /dev/xvda2, /dev/xvda3) на реальном железе будет отличаться (/dev/sda, /dev/sda1, /dev/sda2, /dev/sda3)*

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
  
* Размечаем раздел под swap (1 гигабайт)
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

* Размечаем раздел под root (оставшееся место, 11,9 гигабайт)
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
  
### Получение установочных файлов (ssh)

* Скачиваем установочные файлы. Последняя версия на момент установки: stage3-amd64-20161117.tar.bz2
  ```bash
  livecd ~ # cd /mnt/gentoo
  livecd ~ # wget -c http://gentoo.ussg.indiana.edu/releases/amd64/autobuilds/current-stage3-amd64/stage3-amd64-20161117.tar.bz2
  ```
  
* Распаковываем
  ```bash
  livecd gentoo # tar xvjpf stage3-*.tar.bz2
  ```

* Выбираем ближайшее зеркало для установки
  ```bash
  livecd gentoo # mirrorselect -i -o >>/mnt/gentoo/etc/portage/make.conf
  ```
  
* Копируем файл с адересами днс-серверов
  ```bash
  livecd gentoo # cp -L /etc/resolv.conf /mnt/gentoo/etc/
  ```

### Монтирование слежебных файловых систем (ssh)

* Для доступа к устройствам компьютера после смены корня системы необходимо смонтировать файловые системы /dev, /sys и /proc в корень устанавливаемой системы
  ```bash
  livecd gentoo # mount -t proc none /mnt/gentoo/proc
  livecd gentoo # mount --rbind /sys /mnt/gentoo/sys
  livecd gentoo # mount --rbind /dev /mnt/gentoo/dev
  ```

### Смена корня файловой системы (ssh)

* Меням корень фаловой системы. Точка монтирования `/mnt/gentoo`
  ```bash
  livecd gentoo # chroot /mnt/gentoo /bin/bash
  ```

* Перегружаем настройки профиля
  ```bash
  livecd gentoo # source /etc/profile
  ```
  
* Меняем приглашение командной строки
  ```bash
  livecd gentoo # export PS1="(chroot) $PS1"
  (chroot) livecd / #
  ```

### Установка снимков дерева Portage (ssh)

* Устанавливаем снимок дерева Portage
  ```bash
  (chroot) livecd / # mkdir /usr/portage
  (chroot) livecd / # mkdir -p /usr/portage/profiles
  (chroot) livecd / # echo «gentoo» > /usr/portage/profiles/repo_name
  (chroot) livecd / # emerge-webrsync
  ```

* Обновляем снимок дерев
  ```bash
  (chroot) livecd / # emerge --sync
  ```

### Выбор профиля установки (ssh)

* Выбираем профиль для установки `[1]` из предлoженных
  ```bash
  (chroot) livecd / # eselect profile list
  Available profile symlink targets:
    [1]   default/linux/amd64/13.0 *
    [2]   default/linux/amd64/13.0/selinux
    [3]   default/linux/amd64/13.0/desktop
    [4]   default/linux/amd64/13.0/desktop/gnome
    [5]   default/linux/amd64/13.0/desktop/gnome/systemd
    [6]   default/linux/amd64/13.0/desktop/kde
    [7]   default/linux/amd64/13.0/desktop/kde/systemd
    [8]   default/linux/amd64/13.0/desktop/plasma
    [9]   default/linux/amd64/13.0/desktop/plasma/systemd
    [10]  default/linux/amd64/13.0/developer
    [11]  default/linux/amd64/13.0/no-multilib
    [12]  default/linux/amd64/13.0/systemd
    [13]  default/linux/amd64/13.0/x32
    [14]  hardened/linux/amd64
    [15]  hardened/linux/amd64/selinux
    [16]  hardened/linux/amd64/no-multilib
    [17]  hardened/linux/amd64/no-multilib/selinux
    [18]  hardened/linux/amd64/x32
    [19]  hardened/linux/musl/amd64
    [20]  hardened/linux/musl/amd64/x32
    [21]  default/linux/uclibc/amd64
    [22]  hardened/linux/uclibc/amd64
  (chroot) livecd / # eselect profile set 1
  ```

### Редактируем make.conf (ssh)

* Основные опции сборки системы храняться в файле `/etc/portage/make.conf`. `
* Файл make.conf по умолчанию
  ```bash
  ---------файл make.conf------
  ```
  
  Значение переменных
  ```
  CFLAGS и CXXFLAGS - параметры оптимизации компилятора gcc для языков C (CFLAGS) и C++ (CXXFLAGS)
  CHOST - указывает компилятору gcc для какой архитектуры процессора собирать код
  USE - указывает глобальные опции сборки ПО, полный список команда # less /usr/portage/profiles/use.desc
  PORTDIR - расположение дерева Portage
  DISTDIR - каталог для хранения сжатых исходных кодов
  PKGDIR - каталог для хранения сжатых установочных бинарных пакетов
  ```
  
* Редактируем файл make.conf `nano /etc/portage/make.conf` и дописываем следующие флаги `USE="-X -gtk -gtk2 -qt -qt4 -gnome -kde -xinetd unicode bindist"`. Это флаги отключат поддержку X сервера, xinetd, запретит собирать библиотеки для kde и gnome и добавит поддержку unicode.
* Добавляем переменную `MAKEOPTS="-j3"` (на виртуальную машину выделено два ядра). Предназначена она для контроля запускаемых процессов компиляции при сборке пакета. Рекомендуется устанавливать ее значение исходя из количества ядер процессора плюс 1.
* Файл `make.conf` после редактирования
  ```bash
  # These settings were set by the catalyst build script that automatically
  # built this stage.
  # Please consult /usr/share/portage/config/make.conf.example for a more
  # detailed example.
  CFLAGS="-O2 -pipe"
  CXXFLAGS="${CFLAGS}"
  # WARNING: Changing your CHOST is not something that should be done lightly.
  # Please consult http://www.gentoo.org/doc/en/change-chost.xml before changing.
  CHOST="x86_64-pc-linux-gnu"
  # These are the USE and USE_EXPAND flags that were used for
  # buidling in addition to what is provided by the profile. 
  USE="-X -gtk -gtk2 -qt -qt4 -gnome -kde -xinetd unicode bindist"
  PORTDIR="/usr/portage"
  DISTDIR="${PORTDIR}/distfiles"
  PKGDIR="${PORTDIR}/packages"
  MAKEOPTS="-j3"

  GENTOO_MIRRORS="http://gentoo.c3sl.ufpr.br/ ftp://gentoo.c3sl.ufpr.br/gentoo/   ftp://xeon.gentoo.ru/mirrors/gen$
  ```

### Настройки времени и локали (ssh)

* Выбираем часовой пояс `# ls /usr/share/zoneinfo`, находим свое местоположение и копируем
  ```bash
  (chroot) livecd / # cp /usr/share/zoneinfo/Europe/Moscow /etc/localtime
  (chroot) livecd / # echo "Europe/Moscow" > /etc/timezone
  ```
  
* В файл `/etc/locale.gen` добавляем локаль
  ```bash
  (chroot) livecd / # echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
  (chroot) livecd / # echo "ru_RU.UTF-8 UTF-8" >> /etc/locale.gen
  ```
  
* Генерируем 
  ```bash
  (chroot) livecd / # locale-gen
    * Generating locale-archive: forcing # of jobs to 1
    * Generating 2 locales (this might take a while) with 1 jobs
    *  (1/2) Generating en_US.UTF-8 ...                                            [ ok ]
    *  (2/2) Generating ru_RU.UTF-8 ...                                            [ ok ]
    * Generation complete
  ```
  
### Установка ядра (console)

* Устанавливаем исходные коды ядра
  ```bash
  (chroot) livecd / # emerge gentoo-sources
  ```
* Собираем и устанавливаем ядро genkernel
  ```bash
  (chroot) livecd / # emerge genkernel
  (chroot) livecd / # genkernel all
  ```

---
**NOTE**
Установка ядра через ssh зависла и была перезапущена в консоли. При этом объем оперативной памяти выделеный на виртуальную машину был увеличен до 1Gb и количество ядер 4. Переменная `MAKEOPTS="-j3` не изменялась.

---

### Установка программ (console)

* Устанавливаем необходимый набор программ
  ```bash
  (chroot) livecd / # emerge udev syslog-ng grub dhcpcd vixie-cron
  ```
  
   * udev - менеджер устройств для новых версий ядра Linux, подробнее [тут](https://ru.wikipedia.org/wiki/Udev)
   * syslog-ng - открытая реализация протокола Syslog для Unix и Unix-подобных систем, подробнее [тут](https://redhat-club.org/2011/%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-syslog-ng-%D0%B4%D0%BB%D1%8F-%D1%86%D0%B5%D0%BD%D1%82%D1%80%D0%B0%D0%BB%D0%B8%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B3%D0%BE-%D1%81%D0%B1%D0%BE%D1%80%D0%B0-%D0%B8-%D1%85%D1%80%D0%B0%D0%BD%D0%B5%D0%BD%D0%B8%D1%8F-%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%BD%D1%8B%D1%85-%D1%81%D0%BE%D0%B1%D1%8B%D1%82%D0%B8%D0%B9)
   * grub - загрузчик операционной системы, подробнее [тут](https://ru.wikipedia.org/wiki/GNU_GRUB)
   * dhcpcd — свободная реализация клиента DHCP и DHCPv6. На данный момент является наиболее развитым DHCP-клиентом с открытым исходным кодом, подробнее [тут](http://roy.marples.name/projects/dhcpcd/index)
   * vixie-cron одна из реализаций программы cron, подробнее [тут](https://wiki.gentoo.org/wiki/Cron/ru#vixie_cron)

* Добавляем `udev` в автозагрузку
  ```bash
  (chroot) livecd / # rc-update add udev boot
  ```

### Редактируем fstab (console)

* В файле `fstab` указываем точки монтрования файловых систем в соответствии с разметкой диска
  ```bash
  ----файл fstab---
  ```
  
  
  


  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  