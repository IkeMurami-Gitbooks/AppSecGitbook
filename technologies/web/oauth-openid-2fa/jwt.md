# JWT

Непроверенная инфа: jwt нельзя отзывать, потому что на сервере они не хранятся

JWT: [https://research.securitum.com/jwt-json-web-token-security/](https://research.securitum.com/jwt-json-web-token-security/)

![](<../../../.gitbook/assets/изображение (15).png>)

Еще кейс: пусть у нас `alg` установлен в какой-нибудь ассиметричный алгоритм. Меняем алгоритм на `HS256` и используем открытый ключ как секрет. Пример: [https://hackerone.com/reports/1210502](https://hackerone.com/reports/1210502)

## Время жизни токенов

Access Token — от 5 минут до суток

Refresh Token — до месяца

## Some tips & tools

JWT Security Testing\
[https://mazinahmed.net/blog/breaking-jwt/](https://mazinahmed.net/blog/breaking-jwt/)

Пример проверок на JWT Security на Python\
[https://snikt.net/blog/2019/05/16/jwt-signature-vs-mac-attacks/](https://snikt.net/blog/2019/05/16/jwt-signature-vs-mac-attacks/)

Декодер - [https://jwt.io/](https://jwt.io)
