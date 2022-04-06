# Access Token Reponse

## Successful Response

Если запрос для получения access-токена корректен, то серверу авторизации необходимо сгенерировать access-токен (и опционально refresh-токен) и вернуть его клиенту.

Ответ содержит:

| Parameters                | Description                                                                                                                                             |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| access\_token (required)  |                                                                                                                                                         |
| token\_type (required)    | Обычно, `bearer`                                                                                                                                        |
| expires\_in (recommended) |                                                                                                                                                         |
| refresh\_token (optional) | Токены, полученные через impicit grant, не могут получить refresh-токен в ответ                                                                         |
| scope (optional)          | Если scope совпадает с тем, что запросило приложение, то этот параметр необязателен. Если, например, пользователь изменил scope, то параметр обязателен |

Когда приходит ответ с access-токеном, сервер обязан так же добавить заголовки `Cache-Control: no-store` и `Pragma: no-cache`, чтобы на стороне клиента ответ не закэшировался.

## Unsuccessful Response

Если запрос access-токена некорректен, сервер авторизации должен вернуть ответ об ошибке.

Стутус ответа: 400

| Parameters                    | Description |
| ----------------------------- | ----------- |
| error (required)              |             |
| error\_description (optional) |             |
| error\_uri (optional)         |             |

Error:

* `invalid_request` - в запросе не было необходимых параметров, или запрос содержал неподдерживаемые или повторяющиеся параметры
* `invalid_client` - клиент не прошел аутентификацию (Status: 401)
* `invalid_grant` - authorization code или пароль некорректны или просрочены. Или Redirect URI не совпадает с ранее указанным
* `invalid_scrope`
* `unauthorized_client` - клиент не авторизован на использование указанного grant
* `unsupported_grant_type`
