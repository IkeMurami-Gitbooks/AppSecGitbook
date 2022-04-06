# Client Credentials

## About

Client Credentials grant используется, когда приложение хочет запросить свои собственные ресурсы, а не ресурсы пользователя.

## Parameters

| Parameters                       | Description                                                                    |
| -------------------------------- | ------------------------------------------------------------------------------ |
| grant\_type (required)           | Должен быть установлен в `client_credentials`                                  |
| scope (optional)                 |                                                                                |
| Client Authentication (required) | Через параметры `client_id` и `client_secret` или через HTTP Basic Auth Header |
