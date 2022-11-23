# Other tools

Tool for Web CVE check (Python): [https://github.com/foospidy/web-cve-tests](https://github.com/foospidy/web-cve-tests)

CMS Version Check: [WhatCMS](https://whatcms.org/?s=URL). Обертка на Python с поддержкой API-ключей и оформленная в виде пакета — pywhatcms (Python): [https://github.com/HA71/pywhatcms](https://github.com/HA71/pywhatcms)

Security checks for http headers and cookies (Go)[: https://github.com/gocaio/sct](https://github.com/gocaio/sct)\
usage: sct -url [https://google.com](https://google.com)

Проверка бинарей на то, правильно ли они скомпилены (Bash)\
[https://github.com/slimm609/checksec.sh](https://github.com/slimm609/checksec.sh)

gitleaks (Go) - проверка гит-репозитория на наличие параметров и тп. git: [https://github.com/zricethezav/gitleaks](https://github.com/zricethezav/gitleaks)

wappalyzer - узнаем на каких технологиях собран сайт. Go порт: [https://github.com/rverton/webanalyze](https://github.com/rverton/webanalyze)

transformations (JS) - подаем на вход payload и как его обрабатывает страница; тулза выдает какие преобразования были произведены (пытается предсказать): [https://github.com/jobertabma/transformations](https://github.com/jobertabma/transformations)

dnsobserver (Go) - считай, свой коллаборатор. Работает следующим образом: помогает настроить DNS сервер, внедрить пэйлоад для резолва dns имени сервера. В случае детекта резолва, отправляет уведомление в slack - [https://github.com/allyomalley/dnsobserver](https://github.com/allyomalley/dnsobserver)

[adminer](https://www.adminer.org/) - одностраничный php-скрипт, который позволяет подключаться и  администрировать различные базы данных.

Rogue-MySQL-Server (Python/PHP). git: [https://github.com/allyshka/Rogue-MySql-Server](https://github.com/allyshka/Rogue-MySql-Server)

reNgine (Python) вроде aquatone и др инструментов для сбора информации [https://github.com/yogeshojha/rengine](https://github.com/yogeshojha/rengine)

## Search web apps

httprobe (Go) — поиск http сервисов [https://github.com/tomnomnom/httprobe](https://github.com/tomnomnom/httprobe)

fprobe (Go) — (переписанный httprobe) пробует работать с доменом как с http-сервером. Используется для перечисления скопа. Git : [https://github.com/theblackturtle/fprobe](https://github.com/theblackturtle/fprobe)

httpx (Go) — еще один подобный инструмент. git: [https://github.com/projectdiscovery/httpx](https://github.com/projectdiscovery/httpx)

Usage:

<pre><code><strong>cat hosts.txt | docker run -i projectdiscovery/httpx</strong></code></pre>

## Search hiding params

arjun (Python) - работает хорошо - поиск параметров для get post запросов [https://github.com/s0md3v/Arjun](https://github.com/s0md3v/Arjun)

ParamMiner — плагин бурпа для поиска заголовков, параметров и тп

x8 (Rust) — ([наиболее перспективна из предложенных](https://t.me/webpwn/339)) поиск параметров [https://github.com/sh1yo/x8](https://github.com/sh1yo/x8)

```
x8 -u "https://example.com/" -w <wordlist>
```

ParamPamPam (Python) - то ж для перебора параметров
