# HSTS

HTTP Strict Transport Security — HSTS — Это технология на клиенте (н-р, браузер), которая заворачивает все http-запросы на https (не работает при первичном обращении). То есть, при первом обращении по https к сайту, браузер смотрит заголовок Strict-Transport-Security

```
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

И сохраняет его в кэш. При последующих обращениях к этому ресурсу браузер будет смотреть в кеш и перенаправлять автоматом все запросы с http на https версию сайта.

Еще нюанс: у браузера есть список preload-доменов, для которых HSTS включается сразу (без первого HTTP-запроса).

Защищает этот механизм от пассивных и части активных атак. MiTM осложняется.
