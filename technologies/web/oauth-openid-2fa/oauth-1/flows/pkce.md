# PKCE

PKCE — The Proof Key for Code Exchange (PKCE, pronounced pixie) — расширение протокола, описывающее технику, позоляющую противостоять последствиям перехвата authorization code.

Link: [RFC7637](https://tools.ietf.org/html/rfc7636).

## Authorization Request

Когда нативное приложение начинает делать зарпос авторизации, вместо запуска браузера, клиент сначала создает параметр `code verifier`. Это криптографически случайная строка над алфавитом `A-Za-z0-9\-._~` длиною от 43 до 128 символов. Как приложение сгенерит `code verifier`, оно использует его для создания `code challenge` — `Base64(SHA256(code_verifier))`. Для клиентов, что не могут использовать SHA256, допустимо использовать `code verifier` строку как `code challenge`.

В итоге получаем запрос авторизации со следующими параметрами:

| Parameter               | Description                                       |
| ----------------------- | ------------------------------------------------- |
| `response_type`         | `code`                                            |
| `client_id`             |                                                   |
| `redirect_uri`          |                                                   |
| `state`                 | Случайная строка, которую будем проверять позднее |
| `code_challenge`        |                                                   |
| `code_challenge_method` | `plain` или `S256`                                |

Сервер авторизации, должен распознать наличие параметра `code_challenge` и ассоциировать его с `authorization_code`, который он сгенерит. Если сервер сохраняет authorization code и code challenge в базу, то он может не возвращать challenge обратно. Если у нас self-encoded authorization code, то можно challenge засунуть внутрь authorization code.

Сервер авторизации должен требовать использование PKCE для Public Clients.

## Authorization Code Exchange

Для получения access-токена, клиент отправит вместе с `authorization code` заодно и `code verifier`.

Параметры запроса:

| Parameter       | Description          |
| --------------- | -------------------- |
| `grant_type`    | `authorization_code` |
| `code`          |                      |
| `client_id`     |                      |
| `redirect_uri`  |                      |
| `code_verifier` |                      |

Сервер должен по `authorization_code` найти соотв ему `code_challenge` и `code_challenge_method` и проверить для них `code_verifier`.

PKCE не создает никакие доп запросы, соотв клиенты могут всегда его использовать, даже если сервер не поддерживает это расширение.
