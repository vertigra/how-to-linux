# [mntent]: warning: no final newline at the end of /etc/fstab

> OC: CentOs 6.8 (Final)

**Симптомы**: при загрузке системы появляется сооббщение `[mntent]: warning: no final newline at the end of /etc/fstab` при этом файловые системы нормально монтируются.
**Решение**: добавить `newline` в `/etc/fstab` (перейти в конец последней строчки файла /etc/fstab и нажать `enter`).