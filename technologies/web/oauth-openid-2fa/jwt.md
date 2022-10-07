# JWT

Непроверенная инфа: jwt нельзя отзывать, потому что на сервере они не хранятся

JWT: [https://research.securitum.com/jwt-json-web-token-security/](https://research.securitum.com/jwt-json-web-token-security/)

![](<../../../.gitbook/assets/изображение (15).png>)

Еще кейс: пусть у нас `alg` установлен в какой-нибудь ассиметричный алгоритм. Меняем алгоритм на `HS256` и используем открытый ключ как секрет. Пример: [https://hackerone.com/reports/1210502](https://hackerone.com/reports/1210502)

## Время жизни токенов

Access Token — от 5 минут до суток

Refresh Token — до месяца

## Some Notes & tools

JWT Security Testing\
[https://mazinahmed.net/blog/breaking-jwt/](https://mazinahmed.net/blog/breaking-jwt/)

Пример проверок на JWT Security на Python\
[https://snikt.net/blog/2019/05/16/jwt-signature-vs-mac-attacks/](https://snikt.net/blog/2019/05/16/jwt-signature-vs-mac-attacks/)

Декодер - [https://jwt.io/](https://jwt.io/)

Про токены, JSON Web Tokens (JWT), аутентификацию и авторизацию. Token-Based Authentication: [https://gist.github.com/zmts/802dc9c3510d79fd40f9dc38a12bccfc](https://gist.github.com/zmts/802dc9c3510d79fd40f9dc38a12bccfc)

## Attacks

### Key Confusion Attack

Исходные: JWT токен, поддерживает HS и RS алгоритмы подписи.

Находим публичный ключ (например, в `/.well-known/jwks.json`), кодируем его в нужный формат, переставляем алгоритм подписи в HS и подписываем на этом ключе как на секрете.&#x20;

С какой-то долей вероятности, на стороне сервера, в качестве секрета (по заголовку) возьмется открытый ключ, а в коде на этапе проверки посмотрят на заголовок alg и проверят на этом секрете — PROFIT.

Lab of PortSwigger: [https://portswigger.net/web-security/jwt/algorithm-confusion](https://portswigger.net/web-security/jwt/algorithm-confusion)

