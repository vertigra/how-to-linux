# wget: The certificate of ... is not trusted
*OC: CentOs 6.8 (Final)*

Что бы wget не выдавал ошибку:
```bash
ERROR: The certificate of `www.dropbox.com' is not trusted.
ERROR: The certificate of `www.dropbox.com' hasn't got a known issuer.
```

надо сделать так:
```bash
# apt-get install ca-certificates
```

[Ветка на StackOverflow](http://stackoverflow.com/questions/9224298/how-do-i-fix-certificate-errors-when-running-wget-on-an-https-url-in-cygwin)

