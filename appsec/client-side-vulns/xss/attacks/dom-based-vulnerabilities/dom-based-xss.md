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
