# Про процессы и контроли

## 1

* Asset management: сканим, регулярно валидируем
  * Список систем
  * Список внешних интеграций (куда ходим мы и кто ходит к нам)
  * Балансеры, jump-хосты, прокси
  * Консоли (админки)
  * Базы и другие хранилища с данными
* Документация, описание сервисов
* Контроль доступов (сетевые и логические/ролевые/...)
  * ssh
  * уз внешних и внутренних сервисов (например, DNS провайдеры, серты)
  * секреты прод
* Управление секретами и их ротация
* DWH
* Настройка мониторинга, audit trail
* Настройка контролей учеток и сервисов

## Web

* CI/CD (кто и как ходит на прод)
* Балансеры (серты, домены, балансеры уровня сети и уровня приложения)
* SRE/Admins

## Backend

* Защита от произвольного исполнения кода
* Защита от DoS
  * Защита файловой системы — приложение не должно повлиять на другие приложения, работающие на том же хосте, или на другие процессы
    * readonly фс
    * tmpfs
    * фс поверх файла
