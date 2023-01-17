# Банковские карты и платежи (cards & payments & processing)

Вулна, как-то связанная с POS-терминалом (смогли в QIWI, не зная PIN-кода, сделать операции): [https://hackerone.com/reports/890747](https://hackerone.com/reports/890747)

Вулна при встраивании Apple Pay на сайте и в мобильном приложении. Суть: принимались криптограммы с дргого сайта: [https://hackerone.com/reports/996540](https://hackerone.com/reports/996540)

Как работает 3DS: [https://www.gpayments.com/about/3d-secure/](https://www.gpayments.com/about/3d-secure/)

Виды платежей:

* Двустадийный платеж: через web-форму (вводим все данные карты), средства замораживаются (холд) до подтверждения
* Одностадийный платеж: через 3DS
* h2h — host2host
  * Платеж инициализируется и проводится через бэкенд-сервер целиком (например, работа с подписками — рекурентные платежи)
  * мобильное api

Термины:

* Корзина — информация о платеже и его статус
* Клиринг — процесс списания средств со счета для двустадийного платежа (для одностадийного не применимо)
*

{% file src="../.gitbook/assets/20_лет_проблем_приема_платежей.pdf" %}

## Доклады

* OFFZONE 2018: Про типы платежей (процессинг) [https://www.youtube.com/watch?v=nEqQbDsdPzw](https://www.youtube.com/watch?v=nEqQbDsdPzw)
* OFFZONE 2018: Про ДБО [https://www.youtube.com/watch?v=Fd1aJohHx2Q](https://www.youtube.com/watch?v=Fd1aJohHx2Q)
* OFFZONE 2018: Про процессинг и POS [https://youtu.be/An-x5ZhXnrk](https://youtu.be/An-x5ZhXnrk)
* OFFZONE 2022: Уязвимости в платежных приложениях: [https://youtu.be/wlRRIpADbS8](https://youtu.be/wlRRIpADbS8)
*
