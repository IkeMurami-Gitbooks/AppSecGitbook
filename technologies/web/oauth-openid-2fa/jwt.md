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

Токен может быть подписан с использованием асимметричных алгоримов (например, rsa или ec). В этом случае, проверка токена осуществляется по public key, который может быть указан в заголовке или хранится на сервере в `jwks.json` параметре (а в заголовке `jku` ссылка на этот файл).&#x20;

Мы можем попробовать подписать токен на своем ключе и добавить информацию о публичном ключе в jwk параметр в заголовок токена или указать ссылку на свой сервер, где расположили `jwks.json` файл.

Вообще, сервер не должен доверять чужим публичным ключам (список публичных ключей должен храниться в белом списке каком-то), но разработчики ошибаются)

Пример: [https://portswigger.net/web-security/jwt](https://portswigger.net/web-security/jwt), смотри про `Injecting self-signed JWTs via the jwk parameter`.

### Inject into kid header

kid нужен в тех случаях, когда на стороне сервера используются несколько секретов для работы с шифрованием и подписями.

Стандарт JWT не регламентирует, что такое kid и каким образом и где он должен хранится. Как результат, в этом месте поверхность атаки может расширится, а именно:

* Если kid — это файл, то можно попробовать сделать path traversal. Например: `../../../../../../../dev/null` и подписываем токен на null-байте на алгоритме HS256, например.
* Если kid хранится в базе, можно попробовать сделать SQLi.

### JWT Algorithm Confusion Attack

Исходные: JWT токен, поддерживает HS и RS алгоритмы подписи.

Находим публичный ключ (например, в `/.well-known/jwks.json`), кодируем его в нужный формат, переставляем алгоритм подписи в HS и подписываем на этом ключе как на секрете.&#x20;

С какой-то долей вероятности, на стороне сервера, в качестве секрета (по заголовку) возьмется открытый ключ, а в коде на этапе проверки посмотрят на заголовок alg и проверят на этом секрете — PROFIT.

Lab of PortSwigger: [https://portswigger.net/web-security/jwt/algorithm-confusion](https://portswigger.net/web-security/jwt/algorithm-confusion)

Пример: [https://hackerone.com/reports/1210502](https://hackerone.com/reports/1210502)

### Другие интересные заголовки, приводящие к атакам

#### cty

cty (Content Type) - Sometimes used to declare a media type for the content in the JWT payload. This is usually omitted from the header, but the underlying parsing library may support it anyway. If you have found a way to bypass signature verification, you can try injecting a `cty` header to change the content type to `text/xml` or `application/x-java-serialized-object`, which can potentially enable new vectors for [XXE](https://portswigger.net/web-security/xxe) and [deserialization](https://portswigger.net/web-security/deserialization) attacks.

#### x5c

`x5c` (X.509 Certificate Chain) - Sometimes used to pass the X.509 public key certificate or certificate chain of the key used to digitally sign the JWT. This header parameter can be used to inject self-signed certificates, similar to the [`jwk` header injection](https://portswigger.net/web-security/jwt#injecting-self-signed-jwts-via-the-jwk-parameter) attacks discussed above. Due to the complexity of the X.509 format and its extensions, parsing these certificates can also introduce vulnerabilities. Details of these attacks are beyond the scope of these materials, but for more details, check out [CVE-2017-2800](https://talosintelligence.com/vulnerability\_reports/TALOS-2017-0293) and [CVE-2018-2633](https://mbechler.github.io/2018/01/20/Java-CVE-2018-2633).

## Mitigations

См внизу: [https://portswigger.net/web-security/jwt](https://portswigger.net/web-security/jwt)
