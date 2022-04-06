# JSON Hijacking

Суть атаки в том, что, если где-то GET-запросом возвращается JSON данные (массив?) без CSRF защиты, то злоумышленник может поднять свой сайт и сделать запрос от лица пользователя... Короч, какая-то мутная история. Браузеры вроде уже умеют от этого защищаться. Google внедряет какие-то JS-штуки (например: while(1); - приводит к зависанию при попытке отобразить/обработать такие данные или #####BLABLS#### - приводит к ошибке десериализации)

Чуть примеров (надо попробовать это разобрать)\
[https://www.trustwave.com/en-us/resources/blogs/spiderlabs-blog/json-hijacking-demystified/](https://www.trustwave.com/en-us/resources/blogs/spiderlabs-blog/json-hijacking-demystified/)\
[https://habr.com/ru/post/168461/](https://habr.com/ru/post/168461/)
