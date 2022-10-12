# Рекомендуемые заголовки (Headers)

Устанавливать на сервере рекомендуемые заголовки:

* `Content-Security-Policy`
* `Referer-Policy`
* `Permissions-Policy`
* `X-Frame-Options: DENY` (или `SAMEORIGIN` / `ALLOW-FROM ...` в случае необходимости)
* `X-Content-Type-Options: nosniff`
* `X-XSS-Protection: 1; mode=block`

Дополнительные рекомендации по предотвращению данного типа уязвимостей приведены в [документе Mozilla](https://infosec.mozilla.org/guidelines/web\_security).

Подробно в примерах: [https://habr.com/ru/post/317720/](https://habr.com/ru/post/317720/)
