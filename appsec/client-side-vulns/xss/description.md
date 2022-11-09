# Description

## Типы XXS

* Хранимые (stored)
* Отраженные (reflected)
* DOM-based - параметры не проверяясь, отражаются кодом страницы и вызывают XSS. Уязвимость исключительно на стороне клиента, не сервера.

## Импакт

* Можно украсть куки, авторизоваться под другим пользователем
* Сделать запросы от имени другого пользователя (CSRF)
* Redirect on Phishing Site
* Изменить страницу, заставить пользователя повторно вводить пароль

## Mitigation

link: [https://portswigger.net/web-security/cross-site-scripting](https://portswigger.net/web-security/cross-site-scripting)

&#x20;Preventing cross-site scripting is trivial in some cases but can be much harder depending on the complexity of the application and the ways it handles user-controllable data.

&#x20;In general, effectively preventing XSS vulnerabilities is likely to involve a combination of the following measures:

* &#x20;**Filter input on arrival.** At the point where user input is received, filter as strictly as possible based on what is expected or valid input.
* &#x20;**Encode data on output.** At the point where user-controllable data is output in HTTP responses, encode the output to prevent it from being interpreted as active content. Depending on the output context, this might require applying combinations of HTML, URL, JavaScript, and CSS encoding.
* &#x20;**Use appropriate response headers.** To prevent XSS in HTTP responses that aren't intended to contain any HTML or JavaScript, you can use the `Content-Type` and `X-Content-Type-Options` headers to ensure that browsers interpret the responses in the way you intend.
* &#x20;**Content Security Policy.** As a last line of defense, you can use Content Security Policy (CSP) to reduce the severity of any XSS vulnerabilities that still occur.

