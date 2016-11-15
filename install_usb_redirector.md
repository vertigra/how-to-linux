# Как прокинуть USB без IOMMU
*OC: XEN 6.5*

[Что такое IOMMU.](https://ru.wikipedia.org/wiki/IOMMU)

Если аппаратура не поддерживает IOMMU то многочисленые иструкции по прокидыванию портов в Xen работать не будут. [Тут](https://discussions.citrix.com/topic/378774-xenserver-7-usb-passthrough-on-xen-46/) обсуждение, а [это](https://medium.com/@alexander.bazhenov/%D0%BF%D1%80%D0%BE%D0%B1%D1%80%D0%BE%D1%81-usb-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2-%D0%B2-xenserver-50a9b4e8a80a#.u2xieyosg) пример мануала.
Выход - USB over IP. Клиент-серверное приложение которое позволяет прокидывать USB на гостевые машины, установив серверную часть непосредственно на Xen.
Почти все решения платные или со значительными ограничениями. 
Первое рассмотренное решение [USB Redirector](http://www.incentivespro.com/index.htm) - существует только платная версия. Второе [VirtualHere](https://virtualhere.com/home) - триальная версия позволяет прокидывать один USB-порт. Обсуждение на эту тему [здесь.](http://blog.vadmin.ru/2010/04/usb.html)
Бесплатный проект [The USB/IP Project](https://sourceforge.net/projects/usbip/) не рассмтривается так как последний релиз под Linux датирован 13.01.2009.

### USB Redirector

Сделано по [этой](https://www.citrix.com/blogs/2012/02/29/usb-over-network-with-xenserver-6/) инструкции с поправкой на 64-х битную архитектуру сервера.

* Качаем DDK(Driver Development Kit) 6.5.0 https://www.citrix.com/downloads/xenserver/product-software/xenserver-65-standard.html#ctx-dl-eula
* 



