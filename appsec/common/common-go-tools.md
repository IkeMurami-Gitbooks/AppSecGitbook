# Common Go Tools

## Как бы это могло выглядить?

huntersuite.io — пример проекта с вебчиком на стеке тулзовин на Go.

Фронт -> nuclei/jaeles -> инструменты для recon

Как нибудь сюда завести порт сканеры и c3-фреймворки

Добавить pivoting и типо pentera, но без проверок

## Основа

nuclei (Go) — сканер, конфигурируемый yaml правилами

jaeles (Go) - вроде инфры для web сканера. Однако внутри нет ничего. Надо подключать как-то, но пока хз как.

lair-framework (Go) — три года не поддерживается — это платформа коллобарации: есть скрипты-дроны, которым скармливаешь результат работы инструмента (nessus, nmap,..), а они отправляются в lair.&#x20;

armory (python) - уже содержит в себе кучу плагинов для других тулзовен (порядка 20-40). Такой вот враппер вокруг всего и с единой формой вывода информации. Но на сколько там все соблюдается по производительности - хз

## OSINT

Скраппинг документов с сайта и их анализ:&#x20;

goca \
SpiderFoot (Py) - кучу информации из открытых источников собирает. В gitbook item.red есть раздел с автоматизацией над spyderfoot, gopish и другими компонентами\
У меня есть лицензия Maltego на Ya.Диск ) \
theHarvester \
PublicWWW, RiskIQ

Поиск людей: [https://namechk.com/](https://namechk.com) Тулзы слабенькие, но вдруг: userrecon, sherlock

Hunter.io - поиск почт по домену (Free: 50 запросов/мес)

## Crawling & Scrapping

### URL Crawling

* gau - собирает из открытых источников (из инфы по доменам, из архивов сайтов и тп) по домену\

* urlgrab - нормально собирает ссылки со страницы с прогрузкой сайтов

### URL & JS & Subdomains

hakrawler (Go) - находит меньше и хуже. Не прогружает в браузере. Однако ищет и по скриптам через linkfinder.\
ex: hakrawler -url [http://37.18.9.109](http://37.18.9.109) -depth 3 -linkfinder -js -sitemap -robots -insecure

### Tokens Crawling

zile (py)

### Отслеживание изменений js на сайте

JSMon (Python)

### goclone

Работает норм, через go get скачался, но не установился (на маке вроде тож, кривой пакет?) \
Как crawler пойдет, но:

* Не вытаскивает anon js
* Нет уверенности, что рекурсивно обходит - надо смотреть код
* Не отрабатывает beatifiers

То, что делает сайт - круто!

about: clone site install: \
$ brew tap imthaghost/goclone \
$ brew install goclone

usage: goclone&#x20;

### wappalyzer

Вердикт: гавно, нихера не нашел \
Оригинальный как browser ext - лучше \
TODO: проверить, как работают порты на питон и npm

Для работы нужен apps.json, а в репе Wappalyzer его уже нет => не работает Rename apps.json to technologies.json => надо качать technologies.json wget [https://raw.githubusercontent.com/AliasIO/Wappalyzer/master/src/technologies.json](https://raw.githubusercontent.com/AliasIO/Wappalyzer/master/src/technologies.json)

about: \
port wappalyzer install: \
go get -v -u github.com/rverton/webanalyze/cmd/webanalyze&#x20;

init: webanalyze -update \
\# Bug: Надо качать technologies.json&#x20;

usage: webanalyze -hosts domain\_per\_line.txt -crawl 1

## Fuzzing

Перебор доменов на доступность (они фазятся): dnsmorph \
Есть еще куча интересных тулзовин: см в Pentest/AppSec/tools/Other-tools

## DirEnum

перебор доменов, путей, vhost: gobuster, ffuf, meg

## DomainEnum & Network Mapping (Go)

amass [https://github.com/OWASP/Amass](https://github.com/OWASP/Amass) \
theHarvester \
и любые обвязки вокруг (например, aquatone) \
\+waybackurls and assetfinder\
dnsdumbster: [https://dnsdumpster.com/](https://dnsdumpster.com)

## с2

mwr с3 (c++) + merlin (Go; as c2) или CobaltStrike (есть крякнутые версии, но расскидывать ломаные версии у заказчика - такое себе)

Для merlin наверно надо использовать свой payload - meterpreter, beacon, marader,  какие еще ?..&#x20;

## Узкое

CRLF Fuzz: [https://github.com/dwisiswant0/crlfuzz](https://github.com/dwisiswant0/crlfuzz) \
MiTM/Фишинг: [https://github.com/kgretzky/evilginx2](https://github.com/kgretzky/evilginx2) \
Race Condition: [https://github.com/TheHackerDev/race-the-web](https://github.com/TheHackerDev/race-the-web) \
SSRF - есть какие-то инструменты. В тч и на Go. \
Subdomain Takeover - subjack \
SQLi: sqlmap (py) \
XSS: XSStrike (py), XSSHunter (py), domdig (js) \
httprobe, fprobe

Проверка на CDN (при сканировании помогает): [https://github.com/projectdiscovery/cdncheck](https://github.com/projectdiscovery/cdncheck)

Clickjacking PoC generator [https://github.com/nccgroup/clickjacking-poc](https://github.com/nccgroup/clickjacking-poc)

## Phishing

gophish  (Go) - фреймворк для фишинга

Modlishka (Go)

## Другое

Для отлова Blind OOB [https://github.com/projectdiscovery/interactsh](https://github.com/projectdiscovery/interactsh)

реверс прокси, обход 2FA, тестовый фишинг - Modlishka

gowitness - go библиотека для скринов веба

nuclei — сканер уязвимостей. Вроде используется ([https://github.com/projectdiscovery/nuclei](https://github.com/projectdiscovery/nuclei))
