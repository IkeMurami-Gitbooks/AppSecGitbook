# Scopes

Scope - механизм в OAuth 2.0, ограничивающий доступ приложения к аккаунту пользователя. Приложение может запросить один или несколько scopes, об этом будет выведено в скрине уведомления пользователю, и приложению будет выдан access-токен только ограниченному разрешенному пользователем скопу.

OAuth спецификация разрешает серверу авторизации или пользователю изменять скоп в зависимости от каких либо условий (например, разные скопы для веб и мобильного приложений), но на практике это редкий случай.

Набор scop'ов не определены в OAuth.

## Scope Definitions

При построении модели доступа на основе скопов, стоит начать с разделения на read и write доступы.

Доступ может выдаваться на ресурс, на функциональность, на определенный набор данных.

Так же, крайне рекомендуется делать отдельный скоп для ресурсов, связанных с платежами, для ограничения возможного неправомерного использования функциональности платежей и переводов сторонними приложениями. Другой пример — разграничение доступа к лицензионному ресурсу, демо-ресурсу или публично-доступному ресурсу на основе скопов.

Сервер авторизации должен явно показывать пользователю те ресурсы на основе скопов, к которым OAuth-клиент запросил доступ.

## Modification Scope and Checkboxes

Хотя эта функция кажется недостаточно используемой, спецификация OAuth 2.0 явно позволяет серверу авторизации предоставлять токен доступа с меньшей областью действия, чем запросы приложения. Это оставляет место для некоторых интересных возможностей. Например, пользователь может дать разрешение на доступ, но только с измененным им скопом.

