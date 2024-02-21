# MNA Process

MNA — Merge and acquisition — какие шаги надо выполнить перед, во время и после приобретения и слияния компании/сервиса.

## Вопросы

1. ARCH: Общая архитектура сервиса, его компоненты, frontend, backend, сторонние зависимости и тд
2. ARCH: аутентификация и авторизация между компонентами
3. ARCH: аутентификация и авторизация между клиентом и сервером, между пользователем и сервисом
4. ARCH: логирование: что и куда?
5. ARCH: какие данные обрабатываются? Есть ли персональные данные? Есть ли разрешение на обработку ПДн? Обработка платежной информации? Аутентификационные данные?
6. ARCH: характеристики серверов, где хранится информация (databases, s3, ...)
7. ARCH: стороннее ПО
8. Настройка CI/CD&#x20;
9. Настройка тестов
10. Эксперименты на аудитории (MVT-тесты, A/B-тесты)

## Шаги

1. Убрать все сторонние интеграции
2. Оценить связность с сервисом (http, vpn, authn/authz)
3. Доступы к аккаунтам во внешних сервисах (SSO, 2FA)
4. Управление доступами к ресурсам (IAM)
5. Что доступно из интернета (сервисы, порты)
6. Как устроен доступ во внешних контур (jump-хосты, ssh, консоли, ...)
7. Патч-менеджмент
8. Ротация секретов после поглощения
9. Где нужен доступ в интернет
