# Gitlab

## About

Gitlab - достаточно популярный продукт для разработки, благодаря self-hosted решению его часто можно встретить на поддоменах компаний. Помимо того, что в нем также присутствует регистрация без подтверждения email’а (смотрим в предыдущие посты), это еще и отличная возможность собрать информацию о сотрудниках.

## OSINT

### Пользователи

Без аутентификации доступен следующий API метод - gitlab.company.local/api/v4/users/{id}

Найти по имени конкретного пользователя (чтобы найти id): https://gitlab.company.local/api/v4/users?username=myuser

На самом gitlab - эта ручка также доступна, например [https://gitlab.com/api/v4/users/7154957](https://gitlab.com/api/v4/users/7154957):

{"id":7154957,"name":"Bo0oM","username":"webpwn","state":"active","avatar\_url":"[https://secure.gravatar.com/avatar/4e99709ca6b52f78d02cb92a5bc65d85?s=80\u0026d=identicon","web\_url":"https://gitlab.com/webpwn","created\_at":"2020-09-21T17:25:55.046Z","bio":"","bio\_html":"","location":"","public\_email":"","skype":"","linkedin":"","twitter":"@i\_bo0om.ru","website\_url":"https://t.me/webpwn","organization":"","job\_title":"","work\_information":null}](https://secure.gravatar.com/avatar/4e99709ca6b52f78d02cb92a5bc65d85?s=80\u0026d=identicon%22,%22web\_url%22:%22https://gitlab.com/webpwn%22,%22created\_at%22:%222020-09-21T17:25:55.046Z%22,%22bio%22:%22%22,%22bio\_html%22:%22%22,%22location%22:%22%22,%22public\_email%22:%22%22,%22skype%22:%22%22,%22linkedin%22:%22%22,%22twitter%22:%22@i\_bo0om.ru%22,%22website\_url%22:%22https://t.me/webpwn%22,%22organization%22:%22%22,%22job\_title%22:%22%22,%22work\_information%22:null})

Перебирая идентификаторы, можно за короткое время собрать список логинов (и другую информацию о сотрудниках компании.

### Открытые ключи

По логину также можно узнать открытые ключи - [https://gitlab.com/webpwn.keys](https://gitlab.com/webpwn.keys)

### Логины по аватаркам

Отдельного упоминания заслуживает avatar\_url:

”avatar\_url":"[https://secure.gravatar.com/avatar/4e99709ca6b52f78d02cb92a5bc65d85?s=80\u0026d=identicon”](https://secure.gravatar.com/avatar/4e99709ca6b52f78d02cb92a5bc65d85?s=80\u0026d=identicon%E2%80%9D)

Сервис gavatar содержит email в пути к изображению - 4e99709ca6b52f78d02cb92a5bc65d85. Это ни что иное, как md5 от email’а в нижнем регистре.

echo -n "webpwn@bo0om.ru" | md5

4e99709ca6b52f78d02cb92a5bc65d85

А так как у нас скорее всего корпоративный домен, узнать логины по остальным данным и собрать базу программистов компании будет достаточно просто.

### Паблик проекты

Еще прикол: открываешь gitlab — редирект на страницу авторизации. Но, если открыть https://gitlab.somedomain.com/projects — то покажет public проекты.

### Паблик сниппеты

Пример URL (можно найти через Google Dorks): https://gitlab.example.com/snippets/91

## CVE

### CVE-2021-22205

Суть: создаем сниппет с прикрепленным изображением в формате DjVu и получаем RCE.

Hackerone: [https://hackerone.com/reports/1154542](https://hackerone.com/reports/1154542)

Exploit: [https://vulners.com/packetstorm/PACKETSTORM:162970](https://vulners.com/packetstorm/PACKETSTORM:162970)

Уязвимость в ExifTool ?
