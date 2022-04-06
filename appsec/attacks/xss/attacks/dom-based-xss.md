# DOM-based XSS

### Which sinks can lead to DOM-XSS vulnerabilities? <a href="#which-sinks-can-lead-to-dom-xss-vulnerabilities" id="which-sinks-can-lead-to-dom-xss-vulnerabilities"></a>

&#x20;The following are some of the main sinks that can lead to DOM-XSS vulnerabilities:

&#x20;`document.write()`\
&#x20;`document.writeln()`\
&#x20;`document.domain`\
&#x20;`someDOMElement.innerHTML`\
&#x20;`someDOMElement.outerHTML`\
&#x20;`someDOMElement.insertAdjacentHTML`\
&#x20;`someDOMElement.onevent`

&#x20;The following jQuery functions are also sinks that can lead to DOM-XSS vulnerabilities:

&#x20;`add()`\
&#x20;`after()`\
&#x20;`append()`\
&#x20;`animate()`\
&#x20;`insertAfter()`\
&#x20;`insertBefore()`\
&#x20;`before()`\
&#x20;`html()`\
&#x20;`prepend()`\
&#x20;`replaceAll()`\
&#x20;`replaceWith()`\
&#x20;`wrap()`\
&#x20;`wrapInner()`\
&#x20;`wrapAll()`\
&#x20;`has()`\
&#x20;`constructor()`\
&#x20;`init()`\
&#x20;`index()`\
&#x20;`jQuery.parseHTML()`\
&#x20;`$.parseHTML()`
