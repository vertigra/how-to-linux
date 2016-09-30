# Как поднять сервер NFS в CentOS 6.8
*OC: CentOS 6.8(Final)*

```bash
# yum install nfs-utils nfs-utils-lib
# service rpcbind start
# service nfs start
Starting NFS services:                                     [  OK  ]
Starting NFS mountd:                                       [  OK  ]
Starting NFS daemon:                                       [  OK  ]
Starting RPC idmapd:                                       [  OK  ]
```
Редактируем конфигурационный файл NFS сервера `joe /etc/exports`

```bash
/mnt/share 192.168.1.0/24(rw)
, где:
/mnt/share - путь к папке, для которой раздается доступ;
192.168.1.0/24 - подсеть для который будет доступен расшаренный ресурс (можно указать ip клиента);
(rw) - опции, могут быть:

rw –чтение запись(может принимать значение ro-только чтение);

no_root_squash –по умолчанию пользователь root на клиентской машине не будет иметь доступа к разделяемой директории сервера. Этой опцией мы снимаем это ограничение. В целях безопасности этого лучше не делать;

nohide - NFS автоматически не показывает нелокальные ресурсы (например, примонтированые с помощью mount –bind), эта опция включает отображение таких ресурсов;

sync – синхронный режим доступа(может принимать обратное значение- async). sync (async) - указывает, что сервер должен отвечать на запросы только после записи на диск изменений, выполненных этими запросами. Опция async указывает серверу не ждать записи информации на диск, что повышает производительность, но понижает надежность, т.к. в случае обрыва соединения или отказа оборудования возможна потеря данных;

noaccess – запрещает доступ к указанной директории. Может быть полезной, если перед этим вы задали доступ всем пользователям сети к определенной директории, и теперь хотите ограничить доступ в поддиректории лишь некоторым пользователям.

all_squash– подразумевает, что все подключения будут выполнятся от анонимного пользователя

subtree_check (no_subtree_check)- в некоторых случаях приходится экспортировать не весь раздел, а лишь его часть. При этом сервер NFS должен выполнять дополнительную проверку обращений клиентов, чтобы убедиться в том, что они предпринимают попытку доступа лишь к файлам, находящимся в соответствующих подкаталогах. Такой контроль поддерева (subtree checks) несколько замедляет взаимодействие с клиентами, но если отказаться от него, могут возникнуть проблемы с безопасностью системы. Отменить контроль поддерева можно с помощью опции no_subtree_check. Опция subtree_check, включающая такой контроль, предполагается по умолчанию. Контроль поддерева можно не выполнять в том случае, если экспортируемый каталог совпадает с разделом диска;

anonuid=1000– привязывает анонимного пользователя к «местному» пользователю;

anongid=1000– привязывает анонимного пользователя к группе «местного» пользователя.

nfsvers = 2(или nfsvers = 3) - указывает версию протокола. Использется для хостов с несколькими серверами NFS. Если опция не указана, используется самая высокая поддерживаемая ядром. Опция не поддерживает протокол версии NFSv4  



tcp — Specifies for the NFS mount to use the TCP protocol.

udp — Specifies for the NFS mount to use the UDP protocol.

hard or soft — Specifies whether the program using a file via an NFS connection should stop and wait (hard) for the server to come back online, if the host serving the exported file system is unavailable, or if it should report an error (soft).
If hard is specified, the user cannot terminate the process waiting for the NFS communication to resume unless the intr option is also specified.
If soft is specified, the user can set an additional timeo=<value> option, where <value> specifies the number of seconds to pass before the error is reported.
Note
Using soft mounts is not recommended as they can generate I/O errors in very congested networks or when using a very busy server.

intr — Allows NFS requests to be interrupted if the server goes down or cannot be reached.




```