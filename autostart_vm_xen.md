# Автозапуск виртуальных машин
*OC: XEN 7*

Определеяем UUID пула, для которого мы хотим включить Auto Start
```bash
# xe pool-list
```
и копируем **uuid(RO)**.  
Разрешаем автозапуск виртуальных машин
```bash
# xe pool-param-set uuid=(insert UUID(RO)) other-config:auto_poweron=true 
```
Смотрим список виртуальных машин
```bash
# xe vm-list
```
и копируем **uuid(RO)** нужной машины.  
Включаем автостарт для нужной машины
```bash
# xe pool-param-set uuid=(insert UUID(RO)) other-config:auto_poweron=true 
```

