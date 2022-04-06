# About

Это дополнительный уровень для OAuth, добавляющий возможности для идентификации и аутентификации пользователей, что дает сторонним приложениям строить login sessions.

Link: [https://openid.net/developers/specs/](https://openid.net/developers/specs/)

Link: [https://openid.net/connect/](https://openid.net/connect/)

Link: [https://openid.net/specs](https://openid.net/specs)

## ID Tokens (OpenID Connect)

OpenID Connect базируется на концепте ID Tokens. Эти токены содержат информацию о пользователе и о том, что с этим токеном можно сделать после его получения (после аутентификации пользователя). Ожидается, что с этими данными будет работать OAuth клиент.

ID-токены имеют формат JWT.

ID-токены никогда не должны отправляться в API. OAuth access-токены никогда не должны обрабатываться (читаться) OAuth-клиентом.

Параметры внутри ID Token

| Short Name | Full Name | Description                                        |
| ---------- | --------- | -------------------------------------------------- |
| iss        | issue     | Идентификатор сервера, который выдал токен         |
| sub        | subject   | uid for user                                       |
| aud        | audience  | Идентификатор клиента, который этот токен запросил |
| nonce      |           |                                                    |
| exp        |           |                                                    |
| iat        |           |                                                    |
| auth\_time |           |                                                    |
| acr        |           |                                                    |
|            |           |                                                    |
|            |           |                                                    |



## Tools

OpenID Debugger — инструмент, с помощью которого можно легко отлаживать запросы к OpenID Connect серверу

Link: [https://oidcdebugger.com/](https://oidcdebugger.com)
