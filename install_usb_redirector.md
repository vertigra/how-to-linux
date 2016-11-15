# Как прокинуть USB без IOMMU
*OC: XEN 6.5*

[Что такое IOMMU.](https://ru.wikipedia.org/wiki/IOMMU)

Если аппаратура не поддерживает IOMMU то многочисленые иструкции по прокидыванию портов в Xen работать не будут. [Тут](https://discussions.citrix.com/topic/378774-xenserver-7-usb-passthrough-on-xen-46/) обсуждение, а [это](https://medium.com/@alexander.bazhenov/%D0%BF%D1%80%D0%BE%D0%B1%D1%80%D0%BE%D1%81-usb-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2-%D0%B2-xenserver-50a9b4e8a80a#.u2xieyosg) пример мануала.
Выход - USB over IP. Клиент-серверное приложение которое позволяет прокидывать USB на гостевые машины, установив серверную часть непосредственно на Xen.
Почти все решения платные или со значительными ограничениями. 
Первое рассмотренное решение [USB Redirector](http://www.incentivespro.com/index.htm) - существует только платная версия . Второе [VirtualHere](https://virtualhere.com/home)

