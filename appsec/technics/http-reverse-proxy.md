# HTTP Reverse Proxy

## Что это и зачем нужно

Есть два типа HTTP Proxy - Forward Proxy и Reverse Proxy.&#x20;

Forward Proxy используется клиентом для перенаправления траффика (обычно, когда говорят о прокси, имеют именно этот тип ввиду).

Reverse Proxy используется для балансировки траффика между бэкенд-серверами.

Для чего может быть использован Reverse Proxy:

* Балансировка траффика
* Оборачивать старые проекты в TLS
* Создание тестового фишинга (каким образом?)

## Пример проекта

Modlishka (Go)

git: [https://github.com/drk1wi/Modlishka](https://github.com/drk1wi/Modlishka)

Является Reverse Proxy с дополнительными плюшками, такими как: обход 2FA и создание примеров для фишинга
