# Как уменьшить размер установленной системы
> **черновик**

В процессе установки генты на eee pc 900, сразу после компиляции и установки ядра, еще до этапа установки минимального набора программ я обнаружил что места на root разделе катастрофически мало ~ 500 Mb.

Размер разделов определен конфигруцией жесткого диска eee pc
```
Disk /dev/sda: 3.8 GiB, 4034838528 bytes, 7880544 sectors
Disk /dev/sdb: 7.5 GiB, 8069677056 bytes, 15761088 sectors
```

То есть у нас два жестких диска размером 3.8 и 7.5 Gb, что как оказалось катастрофически мало.

Схема разметки разделов такая:
```
/dev/sda: 3.8 GiB
/dev/sda1 swap  1G 82 Linux swap / Solaris
/dev/sda2 /home 2.7G 83 Linux
/dev/sdb: 7.5 GiB
/dev/sdb1 /boot 100M 83 Linux
/dev/sdb2 /     7.2G 83 Linux
```

Make.conf был такой:
```bash
# These settings were set by the catalyst build script that automatically
# built this stage.
# Please consult /usr/share/portage/config/make.conf.example for a more
# detailed example.
CFLAGS="-march=pentium-m -O2 -pipe -fomit-frame-pointer"
CXXFLAGS="${CFLAGS}"
# WARNING: Changing your CHOST is not something that should be done lightly.
# Please consult http://www.gentoo.org/doc/en/change-chost.xml before changing.
CHOST="i686-pc-linux-gnu"
MAKEOPTS="-j2"
# These are the USE and USE_EXPAND flags that were used for
# buidling in addition to what is provided by the profile.
ENABLE="unicode bindist lm_sensors firefox rdesktop minimal usb jpeg bash-completion bzip2 mp3 rar wifi mmx mmxext sse$
DISABLE="-64bit -ipv6 -qt4 -gnome -kde -minimal -nvidia -radeon -altivec -3dfx -old-linux"
USE="${ENABLE} ${DISABLE}"
PORTDIR="/usr/portage"
DISTDIR="${PORTDIR}/distfiles"
PKGDIR="${PORTDIR}/packages"

GENTOO_MIRRORS="rsync://gentoo.bloodhost.ru/gentoo-distfiles http://gentoo.bloodhost.ru/ ftp://gentoo.bloodhos$
```

Утилита `eclean` из пакета `app-portage/gentoolkit-0.3.0.9-r2` на данном этапе мне ничем помочь не смогла.

Тогда было решено пересобрать @world с `USE` флагом `-doc`. Этот флаг должен отключать (или включать если убрать `-`) установку документации. Заодно я добавил флаги `-qt3 -qt4` потому как собираюсь использовать lxde (c gtk3). Так же я добавил `VIDEO_CARDS="intel"`. Так же я изменил `-minimal` на `minimal` потому как это логично.  

После добавления флагов нужно запустить пересборку пакетов командой `# emerge --ask --changed-use --deep @world`. Так как места катастрофически мало, нужно переопределить  переменную `PORTAGE_TMPDIR="/home/tmp"`. Таком образом сборка пакетов будет происходить в пустующем разделе `/home`, а не в `/var/tmp`.

После пересборки пакетов нужно запустить последовательно:
```
# emerge -p --depclean (смотрим пакеты на удаление)
# emerge --depclean (удаляем)
# revdep-rebuild (динмически перелинковываем текущие объекты)
```

Все эти действия не привели ни к какому положительному эффекту - места стало еще меньше. Тогда я решил изменить `CFLAGS="O2"` на `Os` (>про флаги). И удалил `firfox` из `USE` флагов. 

Перезапуск @world закончился ошибкой `sed: couldn't write 27 items to stdout: No space left on device` - закончилось место на root разделе. 

Тогда в дебрях `/usr/src/linux` я нашел два файла `.tmp_vmlinux1` и`.tmp_vmlinux2`, размером под 300 Мб. Для чего они нужны непонятно, поэтому один из них я перенес в /home и удалил (`.tmp_vmlinux1`). 

После чего заново запустил пересборку мира.