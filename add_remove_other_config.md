# Как задать и сбросить параметр other\_config

> OC: XEN 6.5

Задать параметр в other\_config \(например разрешить автостарт виртуальных машин\) так:

```bash
xe pool-param-set uuid=ecee5987-632e-993d-d5f3-0d9afa8c3928 other-config:auto_poweron=true
```

Сбросить параметр так:

```bash
xe pool-param-remove param-name=other-config param-key=auto_poweron uuid=ecee5987-632e-993d-d5f3-0d9afa8c3928
```



