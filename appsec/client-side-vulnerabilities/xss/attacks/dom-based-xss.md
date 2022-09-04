# DOM-based XSS

### Which sinks can lead to DOM-XSS vulnerabilities? <a href="#which-sinks-can-lead-to-dom-xss-vulnerabilities" id="which-sinks-can-lead-to-dom-xss-vulnerabilities"></a>

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
