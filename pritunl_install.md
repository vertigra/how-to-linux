# Как установить Pritunl (Панель управления OpenVPN сервером)
*OC: Debian 8 Jessie*

Все отлично изложено в [документации](https://docs.pritunl.com/v1/docs/installation)

```bash
# joe /etc/apt/sources.list.d/mongodb-org-3.2.list
(add this)
deb http://repo.mongodb.org/apt/debian wheezy/mongodb-org/3.2 main
# joe /etc/apt/sources.list.d/pritunl.list
(add this)
deb http://repo.pritunl.com/stable/apt jessie main
#  apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 42F3E95A2C4F08279C4960ADD68FA50FEA312927
# apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 7568D9BB55FF9E5287D586017AE645C0CF8E292A
# apt-get update
# apt-get install pritunl mongodb-org
# systemctl start mongod pritunl
# systemctl enable mongod pritunl
```
Заходим браузером на 127.0.0.1 или внешний ip сервера (сервис висит на порту 443, дополнительно указывать это не нужно). Что бы устранить ошибку "SEC_ERROR_UNKNOWN_ISSUER" добавляем для сайта иключение безопасности (как это сделать у мозиллы [тут](https://support.mozilla.org/ru/kb/kak-ustranit-oshibku-s-kodom-sec_error_unknown_iss))
Командой `pritunl setup-key` в командной строке сервера получаем ключ инсталяции и копируем его в поле "Enter setup key" веб интерфеса. После окончания установки можно заходить в веб-интерфейс. Пароль и логин по умолчанию `pritunl`.