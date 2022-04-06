# Vulnerabilities

## Потенциальные проблемы

* Подписи НЕ опциональный параметр, openid.sig значение ДОЛЖНО проверяться с каждым OpenID ответом, о должен быть защищен от replay attacks проверкой nonce

**Надо изучать спецификацию**: есть всякие нюансы, связанные с дефолтной обработкой параметра `request_uri_parameter_supported`

## Есть еще такая вулна (из лабы port swigger):&#x20;

Запросили на сервере аутентификации OpenID конфигурацию: `.well-known/openid-configuration`

В этой конфигурации было указано, что доступен эндпоинт для **регистрации** своего приложения на сервере аутентификации

Пример запроса регистрации:

```
POST /connect/register HTTP/1.1
Content-Type: application/json
Host: server.example.com
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJ ...

{
  "application_type": "web",
  "redirect_uris": ["https://client.example.org/callback"],
  "client_name": "My Example",
  "logo_uri": "https://client.example.org/logo.png",
  "subject_type": "pairwise",
  "sector_identifier_uri": "https://example.org/rdrct_uris.json",
  "token_endpoint_auth_method": "client_secret_basic",
  "jwks_uri": "https://client.example.org/public_keys.jwks",
  "contacts": ["ve7jtb@example.org"],
  "request_uris": ["https://client.example.org/rf.txt"]
}
```

RFC7591: [https://tools.ietf.org/html/rfc7591](https://tools.ietf.org/html/rfc7591)\
Openid Connect Registration 1.0: [https://openid.net/specs/openid-connect-registration-1\_0.html#rfc.section.3.1](https://openid.net/specs/openid-connect-registration-1\_0.html#rfc.section.3.1)

### **SSRF**

#### logo\_uri&#x20;

При регистрации в поле **logo\_uri** указал ссылку на внутренний конфиг AWS IAM. В итоге, при авторизации через мое приложение получил AWS SecretAccessKey.

#### jwks\_uri

jwks\_uri — для JSON Web Key Set (JWK). Этот ключ нужен на сервере для проверки JWT подписи (RFC7523).

Регистрируем свой клиент с проставлением своего uri в этот параметр. Когда сервер перейдет к обратке параметра code (ему надо будет проверить client\_assertion поле), то если он сходит на наш сервак — то это ssrf

пример запроса, на котором может стриггернуться ssrf (сервер будет ожидать json в ответ)

```
POST /oauth/token HTTP/1.1
...

grant_type=authorization_code&
code=n0esc3NRze7LTCu7iYzS6a5acc3f0ogp4&
client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer&
client_assertion=eyJhbGci...
```

#### sector\_identifier\_uri

#### request\_uris

## redirect\_uri Session Poisoning

Пусть у нас есть вот такой запрос на получение кода:

![](<../../../../.gitbook/assets/изображение (17).png>)

Как видите, тело запроса не содержит никаких параметров об авторизованном клиенте, что означает, что сервер берет их из сеанса пользователя. Мы даже можем обнаружить это поведение во время тестирования черного ящика.

Атака, основанная на таком поведении, будет выглядеть примерно так:

* Пользователь посещает специально созданную страницу (как при типичном сценарии атаки XSS / CSRF).
* Страница перенаправляется на страницу авторизации OAuth с «доверенным» «client\_id».
* (в фоновом режиме) Страница отправляет скрытый междоменный запрос на страницу авторизации OAuth с «ненадежным» «client\_id» (мы должны иметь возможность зарегистрировать своего клиента для OAuth сервиса), что отравляет сеанс.
* Пользователь утверждает первую страницу, и, поскольку сеанс содержит обновленное значение, пользователь будет перенаправлен на «redirect\_uri» ненадежного клиента.

**Замечание**: при входе через клиент, пользователь должен одобрить этот вход. Если клиент был одобрен ранеее, сервеер может просто перенаправить его сразу. Что бы избежать этого поведения, можно добавлять к URL-запросу авторизации "prompt=consent".&#x20;

## AMR протокол

RFC 8176

Этот протокол отвечает за тип 2FA. Например, в запросе пробуем `acr_values` менять, например, с `otp+password` на `sms+password`, что приведет к проверке через SMS, а не через приложение-аутентификатор.
