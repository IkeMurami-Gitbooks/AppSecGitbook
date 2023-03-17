# Tools

Abuse Trusted Types для DOM XSS: [https://github.com/filedescriptor/untrusted-types](https://github.com/filedescriptor/untrusted-types)

## domdig

Инструмент для поиска DOM-based XSS в SPA.

domdig (JS) - DOM XSS finding on SPA - [https://github.com/fcavallarin/domdig](https://github.com/fcavallarin/domdig)

```
git clone https://github.com/fcavallarin/domdig
cd domdig && npm i && cd ..
node domdig/domdig.js https://example.com/\?search=t
```

## XSSTron

По факту — это JS Browser на базе Electron. В том числе ищет DOM-based XSS

XSSTron — это экстеншн для Chromium для поиска XSS на сайте: [https://github.com/RenwaX23/XSSTRON](https://github.com/RenwaX23/XSSTRON)

## XSStrike

Python-скрипт для поиска rXSS.

XXStrike (Python): [https://github.com/s0md3v/XSStrike](https://github.com/s0md3v/XSStrike)\
required:\
`pip3 install selenium`\
`brew install geckodriver`\
`pip3 install fuzzywuzzy`

```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements
python xsstrike.py -u "https://example.com/?search=t"
```

## XSSHunter

Deprecated: [https://twitter.com/iammandatory/status/1587106717283721217](https://twitter.com/iammandatory/status/1587106717283721217)

Поднимает сервер и закидывает специфичные пэйлоады с OOB техникой.

XSSHunter (JS/Python): \
web: [https://github.com/mandatoryprogrammer/xsshunter](https://github.com/mandatoryprogrammer/xsshunter)\
client (payload crafter): [https://github.com/mandatoryprogrammer/xsshunter\_client](https://github.com/mandatoryprogrammer/xsshunter\_client)

## poJSon

Платный инструмент. Написан на PHP. Помогает генерировать боевые нагрузки для эксплуатации XSS.

Генератор JS payloads в конкретные PoC (php? надо пробовать) [https://redcodelabs.io/pojson.html](https://redcodelabs.io/pojson.html)

## BeEF

Возможно, динозавр. Инструмент для эксплуатации Client-Side уязвимостей. Часто позиционировался как фреймворк для раскрутки XSS.

BeEF (Ruby/JS) - фреймворк для проведения XSS-атак. git: [https://github.com/beefproject/beef](https://github.com/beefproject/beef)



