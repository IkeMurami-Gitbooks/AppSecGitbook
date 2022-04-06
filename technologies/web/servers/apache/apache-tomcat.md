---
description: >-
  Apache Tomcat is a long-lived, open source Java servlet container that
  implements several core Java enterprise specs, namely the Java Servlet,
  JavaServer Pages (JSP), and WebSockets APIs.
---

# Apache Tomcat

## Структура файловой системы сервера

![](<../../../../.gitbook/assets/изображение (22).png>)

* bin — startup, shutdown, and other scripts and executables
* common — common classes that Catalina and web applications can use
* conf — XML files and related DTDs to configure Tomcat
* logs — Catalina and application logs
* server — classes used only by Catalina
* shared — classes shared by all web applications
* webapps — directory containing the web applications
* work — temporary storage for files and directories

webapps папка содержит war-файл и соотв извлеченная папка (war-файл — это запакованный jsp и ассеты)

![](<../../../../.gitbook/assets/изображение (23).png>)

## Фишечки

Tomcat сразу деплоит (позволяет исполнять при обращении) war-файлы, загруженные в директорию веб-приложения (webapps).

Можно использовать на случай, если JSP не сработал

Скрипт для генерации war-файлов — [godofwar](https://github.com/KINGSABRI/godofwar).

Пример использования:

```
$ godofwar -p cmd_geet
```

## CVEs

### dot-dot-semicolon /..;/

`https://domainname/..;`

url: [https://stackoverflow.com/questions/63742004/two-consecutive-dots-in-url-moves-the-url-navigation-one-step-backward](https://stackoverflow.com/questions/63742004/two-consecutive-dots-in-url-moves-the-url-navigation-one-step-backward)

```
https://domainname/..;/manager/html

```

злоумышленник может попытаться зайти в `host-manager` и `application-manager`.
