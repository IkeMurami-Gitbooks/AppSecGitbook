# JWT

Непроверенная инфа: jwt нельзя отзывать, потому что на сервере они не хранятся

JWT: [https://research.securitum.com/jwt-json-web-token-security/](https://research.securitum.com/jwt-json-web-token-security/)

![](<../../../.gitbook/assets/изображение (15).png>)

## Intro

Стандарт JWT крутится вокруг четырех понятий — JWK, JWS, JWE, JWT.

**JWK** — стандарт, описывающий ключи подписи и шифрования

**JWS** — стандарт, описывающий как подписывать информацию и хранить всю нужную информацию. Защищает **целостность** информации при передачи.

**JWE** — стандарт, описывающий как шифровать информацию и хранить всю нужную информацию. Защищает **конфиденциальность** информации при передачи.

**JWT** — стандарт, описывающий как кодировать и декодировать key-value данные в формат для передачи куда-либо.

### Как это выглядит в действии

По сути, у нас есть пара значений key и value:

* JWT: оберни key и value в json и перегони в строку
* JWK: сгенерируй ключ для подписи и/или шифрования
* JWE: зашифруй свои данные (payload)
* JWS: подпиши свои данные (payload)
* JWT: а еще добавь в заголовок необходимую информацию, чтобы другая сторона смогла расширфовать и/или проверить подпись твоего токена

## Время жизни токенов

Access Token — от 5 минут до суток

Refresh Token — до месяца

## Some Notes & Tools

JWT Security Testing\
[https://mazinahmed.net/blog/breaking-jwt/](https://mazinahmed.net/blog/breaking-jwt/)

Пример проверок на JWT Security на Python\
[https://snikt.net/blog/2019/05/16/jwt-signature-vs-mac-attacks/](https://snikt.net/blog/2019/05/16/jwt-signature-vs-mac-attacks/)

Декодер - [https://jwt.io/](https://jwt.io/)

Про токены, JSON Web Tokens (JWT), аутентификацию и авторизацию. Token-Based Authentication: [https://gist.github.com/zmts/802dc9c3510d79fd40f9dc38a12bccfc](https://gist.github.com/zmts/802dc9c3510d79fd40f9dc38a12bccfc)

JSON Object Signing and Encryption (JOSE) — проект, в котором перечислены все возможные параметры JWT и операции над ними: [https://www.iana.org/assignments/jose/jose.xhtml](https://www.iana.org/assignments/jose/jose.xhtml)

Библиотека для работы с JWT, JWS, JWK, ... — [jwcrypto](https://jwcrypto.readthedocs.io/en/latest/).\
Примеры использования можно [найти](https://github.com/latchset/jwcrypto/issues/14) в тестах к этой библиотеки.

## Attacks

### Set alg=none

Пробуем заменить в заголовке параметр `alg` на `none`. В этом случае токен подписывать не надо. Если сервер доверяет такому токену -> можем имперсонироваться под любую другую сессию.

### Key Injection

Токен может быть подписан с использованием асимметричных алгоримов (например, rsa или ec). В этом случае, проверка токена осуществляется по public key, который может быть указан в заголовке или хранится на сервере в `jwks.json` параметре (а в заголовке ссылка на этот файл).&#x20;

Мы можем попробовать подписать токен на своем ключе и добавить информацию о публичном ключе в jwk параметр в заголовок токена или указать ссылку на свой сервер, где расположили `jwks.json` файл.

Вообще, сервер не должен доверять чужим публичным ключам (список публичных ключей должен храниться в белом списке каком-то), но разработчики ошибаются)

Пример: [https://portswigger.net/web-security/jwt](https://portswigger.net/web-security/jwt), смотри про `Injecting self-signed JWTs via the jwk parameter`.

### Key Confusion Attack

Исходные: JWT токен, поддерживает HS и RS алгоритмы подписи.

Находим публичный ключ (например, в `/.well-known/jwks.json`), кодируем его в нужный формат, переставляем алгоритм подписи в HS и подписываем на этом ключе как на секрете.&#x20;

С какой-то долей вероятности, на стороне сервера, в качестве секрета (по заголовку) возьмется открытый ключ, а в коде на этапе проверки посмотрят на заголовок alg и проверят на этом секрете — PROFIT.

Lab of PortSwigger: [https://portswigger.net/web-security/jwt/algorithm-confusion](https://portswigger.net/web-security/jwt/algorithm-confusion)

Пример: [https://hackerone.com/reports/1210502](https://hackerone.com/reports/1210502)
