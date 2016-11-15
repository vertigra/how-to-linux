# Как прокинуть USB без IOMMU
*OC: XEN 6.5*

[Что такое IOMMU.](https://ru.wikipedia.org/wiki/IOMMU)

Если аппаратура не поддерживает IOMMU, то многочисленые иструкции по прокидыванию usb-портов (или pci-устройств) в Xen работать не будут. [Тут](https://discussions.citrix.com/topic/378774-xenserver-7-usb-passthrough-on-xen-46/) обсуждение, а [это](https://medium.com/@alexander.bazhenov/%D0%BF%D1%80%D0%BE%D0%B1%D1%80%D0%BE%D1%81-usb-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2-%D0%B2-xenserver-50a9b4e8a80a#.u2xieyosg) пример мануала.
Решение - USB over IP. Клиент-серверное приложение которое позволяет прокидывать USB на гостевые машины, установив серверную часть непосредственно на Xen или сторонний сервер.
Почти все решения платные или со значительными ограничениями. 
Первое рассмотренное решение [USB Redirector](http://www.incentivespro.com/index.htm) - существует только платная версия. Второе [VirtualHere](https://virtualhere.com/home) - триальная версия позволяет прокидывать один USB-порт (но без ограничения по времени). Обсуждение на эту тему [здесь.](http://blog.vadmin.ru/2010/04/usb.html) Бесплатный проект [The USB/IP Project](https://sourceforge.net/projects/usbip/) не рассматривается так как последний релиз под Linux датирован 13.01.2009.

### USB Redirector

Сделано по [этой](https://www.citrix.com/blogs/2012/02/29/usb-over-network-with-xenserver-6/) инструкции с поправкой на 64-х битную архитектуру xen сервера.

* Качаем DDK(Driver Development Kit) 6.5.0 https://www.citrix.com/downloads/xenserver/product-software/xenserver-65-standard.html#ctx-dl-eula (нужен учетная запись на [citrix.com](https://identity.citrix.com/Utility/STS/Sign-In?ReturnUrl=https://www.citrix.com%2Faccount))
* Распаковываем на той машине где установлен Xen Center (или монтируем образ на виртуальный дисковод).
* Импортируем виртуальную машину с помощью Xen Center (XenServer -> Import...). В locate указать ova.xml (например J:\ddk\ova.xml). После импорта машина инсталируется (в процессе инсталяции будет запрос пароля для пользователя root)
* После логина в консоли DDK делаем следующее:
  ```bash
  # wget http://www.incentivespro.com/usb-redirector-linux-x86_64.tar.gz
  # tar xzvf usb-redirector-linux-x86_64.tar.gz
  # cd usb-redirector-linux-x86_64
  # ./installer.sh install-server
  # cd ..
  # tar czvf  usb-redirector-linux-x86_64.tgz usb-redirector-linux-x86_64 
  # scp usb-redirector-linux-x86_64.tgz root@ip_xen_server:/root
  ```
  после этого виртуальную машину DDK можно выключить.
* В консоли Xen делаем следующее:
  ```bash
  # cd ~
  # tar xzvf usb-redirector-linux-x86_64.tgz
  # cd usb-redirector-linux-x86_64
  ```
  редактируем файл `installer.sh`
  ```bash
  # joe installer.sh
  ```
  В функции `usbsrv_install()` (поиск в joe CTRL+KF) коментируем:
  ```bash
  if [ ! -d $KERNELDIR ]; then
      exit_with_error "Kernel sources or kernel headers directory not found. Please install 
      the  corresponding package first."
  fi
  ```
  В функции usbsrv_make_kernel_module() коментируем:
  ```bash
  make KERNELDIR=$KERNELDIR clean; /dev/null 2>1
  make $make_flags $driver_config KERNELDIR=$KERNELDIR $script_dir/buildlog.txt 2>1
  ```
  Сохраняем и закрываем.
* Запускаем инсталятор `# ./installer.sh install-server`
  ```bash
  *** Installing USB Redirector for Linux v3.6
  ***  Destination dir: /usr/local/usb-redirector
  ***  Checking installation...
  ***  Detecting system...
  ***     distribution: redhat
  ***     kernel: 3.10.0+2
  ***  Compiling kernel module...
  ***  Kernel module successfully compiled
  ***  Creating directories...
  ***  Preparing scripts...
  ***  Copying files...
  ***  Setting up init script...
  ***  Starting daemon...
  ***  Please allow incoming connections on 32032 port for USB Sever to be able to accept  
  connections from remote clients.
  ***  INSTALLATION SUCCESSFUL! To uninstall, run /usr/local/usb-redirector/uninstall.sh
  ```
* Добавляем разрешающее правило в фаервол `joe /etc/sysconfig/iptables`
  
  ```bash
  -A RH-Firewall-1-INPUT -m conntrack --ctstate NEW -m tcp -p tcp --dport 22 -j ACCEPT
  -A RH-Firewall-1-INPUT -m conntrack --ctstate NEW -m tcp -p tcp --dport 80 -j ACCEPT
  -A RH-Firewall-1-INPUT -m conntrack --ctstate NEW -m tcp -p tcp --dport 443 -j ACCEPT
  (add this)
  -A RH-Firewall-1-INPUT -m conntrack --ctstate NEW -m tcp -p tcp --dport 32032 -j ACCEPT
  ```
* Посмотреть подключеные устройства можно так:
  ```bash
  # usbsrv -list 

  ================= USB SERVER OPERATION SUCCESSFUL ===============
  List of local USB devices:

   1: Generic USB K/B Composite USB Device
      Vid: 13ba   Pid: 0017   Port: 2-2
      Status: plugged

   3: WindowsCE CASIO.
      Vid: 07cf   Pid: 3303   Port: 1-1
      Status: plugged

  ===================== ======================= ===================
  ```
  Расшарить так:
  ```bash
  usbsrv -s 3

  ====================== OPERATION SUCCESSFUL =====================
  USB device has been shared
  ===================== ======================= ===================
  ```
* После этого в гостевой машине следует поставить клиента, указать код лицензии и подключиться к запущеному серверу. 