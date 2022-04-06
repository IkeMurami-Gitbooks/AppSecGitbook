# Token Introspection Server Flow

OAuth 2.0 Token Introspection расширение определяет протокол предоставление информации об access-токене для сервера ресурсов или другого внутреннего сервера.

Link: [RFC7662](https://tools.ietf.org/html/rfc7662)

Отдельный эндпоинт может потребоваться для того, что б поделиться базой или client secret.

## Token Information Request

Этот запрос не должен быть доступен конечным пользователям, тк он может содержать конфиденциальную информацию, которая не должна быть доступна разработчикам OAuth-клиентов. Можно сделать этот сервер недоступным извне или защитив его через HTTP Basic Auth:

```
POST /token_info HTTP/1.1
Host: authorization-server.com
Authorization: Basic Y4NmE4MzFhZGFkNzU2YWRhN
 
token=c1MGYwNDJiYmYxNDFkZjVkOGI0MSAgLQ
```

## Token Information Response

Возвращает в виде JSON:

* active (required) — true или false
* scope — json struct
* `client_id`
* username
* exp

Пример:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
  "active": true,
  "scope": "read write email",
  "client_id": "J8NFmU4tJVgDxKaJFmXTWvaHO",
  "username": "aaronpk",
  "exp": 1437275311
}
```

## Security

**Check JWT**: Прежде, чем выдать информацию по токену, сервер должен проверить необходимые характеристики JWT — подпись, время жизни и тд

**Token Fishing**: если на token introspection endpoint не настроен тротлинг и эндпоинт доступен извне, то атакующий может попробовать перебирать токены, пока не найдет активный. Для противодействия этому, сервер должен использовать аутентификацию, или быть недоступным извне, или доступ осуществляться только из-под файервола. Так же, сервер ресурсов может иметь уязвимости, которые затронут и Token Introspection Server.

**Caching**: для повышения производительности, introspection endpoint может быть настроен с поддержкой кэширования. Trade-off security: короткий срок жизни для кэша — большая нагрузка на сервер, долгий срок жизни — после истечения токена этот сервис из-за кэша будет считать, что токен корректен. Один из вариантов решения — выкидывать из кэша значение, если `exp` истек.
