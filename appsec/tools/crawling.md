---
description: >-
  Процесс автоматищированного получения информации (вроде сорцов, копии сайта и
  тп)
---

# Crawling

### .git crawling

DVCS-Ripper (Perl) - Git web crawling [https://github.com/kost/dvcs-ripper](https://github.com/kost/dvcs-ripper)

## Screen Web Crawling

#### Aquatone

git:  [https://github.com/michenriksen/aquatone](https://github.com/michenriksen/aquatone)

Особенности:

* Написан на Go
* Автор говорит, что работает программа  хорошо в связке с: amass, sublist3r, subfinder, fierce, gobuster, knockpy
* Может брать на вход URL/хосты/выход из nmap/masscan

Возможно годится на то, чтобы сканить вебчики и смотреть скрины

Пример использования:&#x20;

```
cat hosts.txt | aquatone -ports 80,443,3000,3001 -out ~/aquatone/example.com
```

Баги: если в -out будут относительные пути — скриншотов не будет!

В папке будут:&#x20;

* скриншоты
* html-страницы/заголовки
* единый репорт, в котором все наглядно
* url'ы всех вебчиков обнаруженных

### gowitness

Библиотека на go для создания скриншотов сайтов [https://github.com/sensepost/gowitness](https://github.com/sensepost/gowitness)

## URL crawling from WebApp

### gau

gau (Go) - собирает из открытых источников (из инфы по доменам, из архивов сайтов и тп) по домену - [https://github.com/lc/gau](https://github.com/lc/gau)

### hakrawler

hakrawler (Go): [https://github.com/hakluke/hakrawler](https://github.com/hakluke/hakrawler)

Нормально ставится через go get

Находит меньше, чем через urlgrab, но зато ищет относительные пути в js (правда, через регулярку из linkfinder). Можно использовать в паре с urlgrab

Usage: `hakrawler -url <URL> -depth 3 -linkfinder -js -sitemap -robots -insecure`

### urlgrab

urlgrab (Go) - нормально собирает ссылки со страницы с прогрузкой сайтов [https://github.com/IAmStoxe/urlgrab](https://github.com/IAmStoxe/urlgrab)\
Собирать проект через make build (собирает круто локально в докере)\
Использовать: `urlgrab -url <url> -output-all /directory/result/out -render-js`-ignore-ssl

### gal

gal (Bash): [https://github.com/YashGoti/gal](https://github.com/YashGoti/gal)

## Tokens & API keys crawling

zile (Python) [https://github.com/xyele/zile](https://github.com/xyele/zile)

## JS change monitoring

JSMon (Python) - мониторим изменение скриптов  на сайте. В случае изменений -> получаем оповещение в телеграмм. [https://github.com/robre/jsmon](https://github.com/robre/jsmon)

## Копирование сайтов

goclone (Go) - выкачиваем сайт: [https://github.com/imthaghost/goclone](https://github.com/imthaghost/goclone)

Замечание по проекту: надо поправить и сделать pull request

```go
// https://github.com/imthaghost/goclone

url := "https://example.com"
parser.ValidateURL(url) && parser.ValidateDomain(url)  // сомнительно, конечно: лучше использовать какую-то др библиотеку
file.CreateProject(urlDomain)  // здесь пздц: потенциально directory path traversal -> исправить. Extractor - отвечает за сохранение информации со страницы в локальную директорию
crawler.Crawl(url, projectpath)  // Использует Colly. Не извлекает анонимные скрипты, оставляет их в html
html.LinkRestructure(projectpath)  // подставляет img, script, css в html + форматирует
exec.Commnad("open", "http://localhost:5000").Start()
server.Serve(projectpath)  // не понятно, как этим пользоваться

```



\
