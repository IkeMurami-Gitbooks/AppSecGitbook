# Description

## CNAME-запись

Получаешь список CNAME записей на DNS сервере. Проверяешь, есть ли среди них ссылки на сторонние сервисы (например, aws s3 bucket). Если есть и оказывается, что домен, в который резолвится CNAME запись, свободен — идешь на соотв сервис и создаешь там проект так, чтобы новый домен совпадал с соотв записью на DNS сервере.

## A-запись

Получаешь список A записей на DNS сервере. Переходишь на все сторонние ip и смотришь, не принадлежат ли они стороннему сервису (например, это vps, или веб-приложение в облачной инфраструктуре).&#x20;

В случае с VPS: если этот IP свободен, то пробуешь создавать VPS (пересоздавать), пока не получишь нужный IP. Тем самым снова угоняем домен

### Tilda

Есть сервис Tilda — сервис создания веб-сайтов. Суть в том, что если переходите на IP, а там такой логотип:&#x20;

![](<../../../../.gitbook/assets/изображение (27).png>)

или

![](<../../../../.gitbook/assets/изображение (31).png>)

Это значит, что веб-приложение для домена куплено, но не развернуто.

Если видим, такой логотип:

![](<../../../../.gitbook/assets/изображение (28).png>)

Значит, мы можем указать домен Заказчика, и он будет резолвится в наш сайт (Tilda вообще срать, какой домен вы напишите). Нужен тариф Tilda Professional. Проекты на тилде располагаются в 185.x.x.x сети