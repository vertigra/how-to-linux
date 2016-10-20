# wget: The certificate of ... is not trusted
*OC: CentOs 6.8 (Final)*

Что бы wget не выдавал ошибку:
```bash
ERROR: The certificate of `www.dropbox.com' is not trusted.
ERROR: The certificate of `www.dropbox.com' hasn't got a known issuer.
```

надо сделать так
```bash
# apt-get install ca-certificates
```
