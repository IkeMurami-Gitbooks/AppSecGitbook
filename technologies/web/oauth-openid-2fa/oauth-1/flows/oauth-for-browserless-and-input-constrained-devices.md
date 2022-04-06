# OAuth for Browserless and Input Constrained Devices

OAuth 2.0 “Device Flow” расширение позволяет использовать OAuth на девайсах, которые имеют выход в интернет, но не могут открыть браузер (например, расширения для TV).

Обычно, девайс говорит пользователю открыть какую-то ссылку на стороннем девайсе (например, на телефоне через чтение QR-кода) и получает access-токен таким образом.

## Authorization Request

Девайс делает запрос к серверу авторизации для получения device code параметра:

```
POST /token HTTP/1.1
Host: authorization-server.com
Content-type: application/x-www-form-urlencoded
 
client_id=a17c21ed
```

Некоторые сервера авторизации позволяют добавлять параметр `scope`, который будет показан пользователю.

Сервер возвращает сссылку, куда перенаправить пользователя и `user_code` для него.

```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
{
    "device_code": "NGU5OWFiNjQ5YmQwNGY3YTdmZTEyNzQ3YzQ1YSA",
    "user_code": "BDWP-HQPK",
    "verification_uri": "https://authorization-server.com/device",
    "interval": 5,
    "expires_in": 1800
}
```

## Token Request

Пока пользователь не подтвердит доступ, девайс будет стучать на сервер авторизации со своим `device_code` с заданным сервером `interval`, пока сервер не вернет статус, отличный от `error = authorization_pending`.

Пример запроса:

```
POST /token HTTP/1.1
Host: authorization-server.com
Content-type: application/x-www-form-urlencoded
 
grant_type=urn:ietf:params:oauth:grant-type:device_code&amp;
client_id=a17c21ed&amp;
device_code=NGU5OWFiNjQ5YmQwNGY3YTdmZTEyNzQ3YzQ1YSA
```

Если пользователь откажет в доступе, сервер вернет `error = access_denied`. Если токен истек — получим `error = expired_token`.

Если все ОК:

```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store
 
{
  "access_token": "AYjcyMzY3ZDhiNmJkNTY",
  "refresh_token": "RjY2NjM5NzA2OWJjuE7c",
  "token_type": "bearer",
  "expires": 3600
}
```

## Security

* `user_code` должен быть не подвержен бруту, для запроса подтверждения доступа со стороны пользователя должен быть установлен rate limit.
