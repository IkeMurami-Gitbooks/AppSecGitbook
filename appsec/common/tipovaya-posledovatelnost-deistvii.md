# Типовая последовательность действий

1. Если есть сорцы -> SAST Checkmarx & Fortify HP
2. OSINT: если применимо
   1. Домены/поддомены
   2. Смотрим по утечкам
   3. Google Dorks
3. Хосты/Порты: masscan/nmap/nessus. Берем с предыдущего шага
4. Внешка: По таргетам - aquatone (вроде норм) для скринов по вебчикам (или другая подобная тулза); Grabber
5. Web: Crawling & Scrapping
   1. Клон сайта (паук): goclone
   2. URL Crawling: gau (не для тестовых стендов), hakrawler, urlgrab
   3. Токены и строки: ?
   4. wapplyzer
6. &#x20;

Домены/поддомены можно и с помощью shodan смотреть - очень хорошо показывает (но в паре с amass куда эффективнее)

у парня bash скрипт, который реализует последовательность действий \
amass -> subfinder -> httprobe -> naabu(nmap на go) -> aquatone -> gau -> dirsearch \
[https://github.com/0xdekster/deksterecon/blob/master/dekster.sh](https://github.com/0xdekster/deksterecon/blob/master/dekster.sh)\
WebGUI: [https://github.com/0xdekster/ReconNote](https://github.com/0xdekster/ReconNote)

IP в DNS и обратно (очень хороший сервис): [https://viewdns.info](https://viewdns.info)\
Reverse IP Lookup — ищет историю DNS-записей для IP\
Reverse DNS Lookup — ищет домены по PTR-записям



Вот есть такая картинка:

![](<../../.gitbook/assets/изображение (25).png>)

## Automation as a Service

Компании, которые этим занимаются (пилят open-source и вокруг этого строят платформы):

* [https://projectdiscovery.io/#/](https://projectdiscovery.io/#/)
* huntersuite.io
