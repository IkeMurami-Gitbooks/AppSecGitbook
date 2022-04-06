# Prototype Pollution Attack

Сборник вулнов: [https://github.com/BlackFan/client-side-prototype-pollution](https://github.com/BlackFan/client-side-prototype-pollution)

Какой-то инструмент: [https://github.com/msrkp/PPScan/](https://github.com/msrkp/PPScan/)

Пример и mitigation для этой атаки: [https://www.whitesourcesoftware.com/resources/blog/prototype-pollution-vulnerabilities/](https://www.whitesourcesoftware.com/resources/blog/prototype-pollution-vulnerabilities/)

Пример:

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
