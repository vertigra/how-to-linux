# Starting NFS statd: [FAILED]

> OC: CentOS 6.8 (Final)

**Симптомы**: при загрузке система виснет на этапе `Starting NFS statd:` после некоторого времени продолжает загрузку, при этом статус запуска NFS демона `[FAILED]` (`Starting NFS statd: [FAILED]`).   
**Решение**: дождаться загрузки ситемы, и добавить службы NFS в автозагрузку `# chkconfig nfs on && chkconfig rpcbind on`