# Получить IP спрятанный за CDN (Search Real IP)

В статье описывается как через CENSYS найти оригинальный ip + даются ссылки на прошлые работы. И описывается как пользоваться CURRYFINGER. Статья: [https://dualuse.io/blog/curryfinger/](https://dualuse.io/blog/curryfinger/)

CURRYFINGER (Go): [https://github.com/tbiehn/CURRYFINGER](https://github.com/tbiehn/CURRYFINGER)

CloudFlair (Python): [https://github.com/christophetd/CloudFlair](https://github.com/christophetd/CloudFlair)

## 6 способов найти реальный IP домена, спрятанного за Cloud Firewall

* Проверить контент сайта  на shodan.io или cencys.io, смотрим за контентом или заголовками
* Попробовать найти стектрейс: пробуем некорректные HTTP методы, параметры, заголовки. IP может быть обнаружено таким образом
* Проверить историю DNS: securitytrails.com, viewdns.info/iphistory
* Проверить SSRF-подобные штуки. Коннектимся к нашему серверу и проверяем IP источника
* Проверить функциональность почтового сервера. Если сайт может отправлять письма (например, при восстановлении пароля), то мы можем посмотреть реальный API отправителя (используя Burp Collaborator)
* Попробовать все выше на дргуих доменах и поддоменах. Может они они не спрятаны за cloud firewall
