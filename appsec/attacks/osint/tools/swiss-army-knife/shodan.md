# Shodan

### Shodan Wrappers Tools

Shodan запросы\
[https://github.com/jakejarvis/awesome-shodan-queries](https://github.com/jakejarvis/awesome-shodan-queries)\
[https://github.com/BullsEye0/shodan-eye](https://github.com/BullsEye0/shodan-eye)\
[https://github.com/HatBashBR/ShodanHat](https://github.com/HatBashBR/ShodanHat)

### Shodan HowTo - небольшой мануал (в Kali Linux)

#### Начало

1\. Установка:\
easy\_install shodan\
2\. Вбиваем наш API ключ из личного кабинета:\
shodan init {api\_key}

#### Полезные команды для работы

1\. Проверить, есть ли хост в базе и какая есть инфа о нем:\
shodan host x.x.x.x\
2\. Поиск определенных хостов (например, с установленным MS IIS 7.0):\
shodan search --fields ip\_str,port,org,hostnames microsoft iis 7.0\
3\. Посчитать количество хостов, удвл критерию\
shodan count Apache/2.4

#### Как использовать Shodan в Metasploit

Можно также использовать поиск по Shodan внутри модуля Metasploit. Для этого существует отдельный модуль shodan\_search.\
Подгружаем модуль в msf:\
`use auxiliary/gather/shodan_search`\
`show options`\
`Задаем наш API_KEY: set SHODAN_APIKEY {key}`\
`Сделать запрос (Для примера найдем роутеры Netgear, с открытой веб-мордой для авторизации):`\
`set QUERY "401 authorization netgear"`\
`run`\
Дальше уже можно получить эти IP, записать их в файл и пройтись например Гидрой на предмет несложных или дефолтных паролей.

Найдем камеры, которые работают webcamxp:\
`set QUERY "webcamxp"`\
`run`

Найдем устройства с Windows 7:\
`set QUERY "Windows 7"`\
`run`

### Статьи по Shodan

https://medium.com/bugbountywriteup/using-shodan-better-way-b40f330e45f6

{% file src="../../../../../.gitbook/assets/Shodan RU.pdf" %}

