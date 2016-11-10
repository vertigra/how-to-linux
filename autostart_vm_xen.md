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

### Автозапуск виртуальных машин с задержкой
*OC: XEN 6.5*

Возможно работает и на XEN 7 - не проверялось.

* Командой ```# xe vm-list``` узнаем uuid(RO) виртуальной машины
  ```bash
  uuid ( RO)           : 2a3f702d-6fb0-0d8c-a670-e124fd7cb6e8
  name-label ( RW): debian.client
  power-state ( RO): halted
  ```

* Прописываем в ```/etc/rc.local``` виртуальные машины которые следует запускать автоматически.
  Параметр ```sleep 20``` - задержка 20 секунд (необязательный). 
  ```bash
  # joe /etc/rc.local
  (add this in end file)
  sleep 20
  xe vm-start uuid=2a3f702d-6fb0-0d8c-a670-e124fd7cb6e8
  ```
  
  Если VM несколько так:
  ```bash
  # joe /etc/rc.local
  (add this in end file)
  #First VM
  sleep 20
  xe vm-start uuid=2a3f702d-6fb0-0d8c-a670-e124fd7cb6e8
  #Second VM
  sleep 40
  xe vm-start uuid=3b4a321d-7dw1-1a9c-a670-e1r23sg5df61
  ```

Можно также использовать ```name-label``` например
```bash
# xe vm-list params | grep name-label
name-label ( RW): centos.server
name-label ( RW): debian.client
```

В ```/etc/rc.local``` следует писать тогда так:
```bash
sleep 20
xe vm-start vm=centos.server
sleep 40
xe vm-start vm=debian.client
```
  
