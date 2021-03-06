# Протокол SIP

## Common

Протокол инициализации сеанса (SIP) позволяет устанавливать связь, завершать или изменять голосовые или видеозвонки. Трафик передается по RTP. SIP - протокол прикладного уровня, который использует UDP/TCP как транспорт.

Порты (по умолчанию)

* Нешифрованный трафик - 5060
* Шифрованный (TLS) трафик - 5061

Протокол инициации сеанса использует ASCII и очень похож на HTTP (использует модель запроса/ответа). Запросы к клиенту SIP осуществляются через SIP URI и AGI через пользовательский агент, аналогично HTTP-запросу веб-браузера.

## SIP-ответы

Мы можем понять ответы, используя код ответа; общие категории ответов:

* 1xx (информационный)
* 2xx (успех)
* 3xx (перенаправление)
* 4xx (неудачные запроосы)
* 5xx (веб-сервер не может выполнить запрос)
* 6xx (глобальные ошибки)

## Структура взаимодействия SIP

Типичный протокол:

* Отправитель инициирует запрос INVITE
* Получатель отправляет ответ 100 (пытается звонить)
* Отправитель начает звонить, отправив ответ 180 (звонок)
* Приемник поднимает трубку, и отправляется успешный ответ 200 (ОК)
* ACK отправляется инициатором
* Вызов начал использовать RTP
* Запрос BYE отправлен для завершения вызова

## RTP: транспортный протокол в реальном времени

RTP - сетевой протокол для передачи аудио и видео по сетям.&#x20;

Порт по умолчанию: 16384 - 32767, эти порты используются для вызовов SIP.&#x20;
