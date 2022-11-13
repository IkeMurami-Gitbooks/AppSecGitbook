# About

Объектная модель документа (DOM) — это иерархическое представление элементов на странице в веб-браузере. Веб-сайты могут использовать JavaScript для управления узлами и объектами модели DOM, а также их свойствами. Манипуляции с DOM сами по себе не являются проблемой. Фактически, это неотъемлемая часть работы современных веб-сайтов. Однако JavaScript, который небезопасно обрабатывает данные, может способствовать различным атакам. Уязвимости на основе DOM возникают, когда веб-сайт содержит код JavaScript, который принимает контролируемое злоумышленником значение, известное как **source**, и передает его опасной функции, известной как **sink**.

Источник: [https://portswigger.net/web-security/dom-based](https://portswigger.net/web-security/dom-based)

### Source

Source — js property, которое принимает данные, контролируемые злоумышленником. Например:

```
document.URL
document.documentURI
document.URLUnencoded
document.baseURI
location
document.cookie
document.referrer
window.name
history.pushState
history.replaceState
localStorage
sessionStorage
IndexedDB (mozIndexedDB, webkitIndexedDB, msIndexedDB)
Database
web messages
```

### Sink

Sink — потенциально опасная функция или DOM-объект, который может иметь неприятный эффект, если туда попадут данные от злоумышленника. Например:

| DOM-based vulnerably                                                                                                | Example sink             |
| ------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| [DOM XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based)                                      | document.write()         |
| [Open redirect](https://portswigger.net/web-security/dom-based/open-redirection)                                    | window.location          |
| [Cookie manipulation](https://portswigger.net/web-security/dom-based/cookie-manipulation)                           | document.cookie          |
| [JavaScript injection](https://portswigger.net/web-security/dom-based/javascript-injection)                         | eval()                   |
| [Document-domain manipulation](https://portswigger.net/web-security/dom-based/document-domain-manipulation)         | document.domain          |
| [WebSocket-URL poisoning](https://portswigger.net/web-security/dom-based/websocket-url-poisoning)                   | WebSocket()              |
| [Link manipulation](https://portswigger.net/web-security/dom-based/link-manipulation)                               | element.src              |
| [Web message manipulation](https://portswigger.net/web-security/dom-based/web-message-manipulation)                 | postMessage()            |
| [Ajax request-header manipulation](https://portswigger.net/web-security/dom-based/ajax-request-header-manipulation) | setRequestHeader()       |
| [Local file-path manipulation](https://portswigger.net/web-security/dom-based/local-file-path-manipulation)         | FileReader.readAsText()  |
| [Client-side SQL injection](https://portswigger.net/web-security/dom-based/client-side-sql-injection)               | ExecuteSql()             |
| [HTML5-storage manipulation](https://portswigger.net/web-security/dom-based/html5-storage-manipulation)             | sessionStorage.setItem() |
| [Client-side XPath injection](https://portswigger.net/web-security/dom-based/client-side-xpath-injection)           | document.evaluate()      |
| [Client-side JSON injection](https://portswigger.net/web-security/dom-based/client-side-json-injection)             | JSON.parse()             |
| [DOM-data manipulation](https://portswigger.net/web-security/dom-based/dom-data-manipulation)                       | element.setAttribute()   |
| [Denial of service](https://portswigger.net/web-security/dom-based/denial-of-service)                               | RegExp()                 |
