# ffuf (Go)

## About

веб фаззер на go. [https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf)

## Basic Usage

ffuf -w wordlist -u https://url/FUZZ -c -v

&#x20;\-c - color\
&#x20;\-v - verbose\
&#x20;\-r - allow redirect\
&#x20;\-o out.json\
&#x20;\-replay-proxy http://127.0.0.1:8888/

Не проверял, но возможно проксируются только успешные запросы (после фильтров)

ffuf -w wordlists/SecLists/Discovery/Web-Content/raft-medium-directories-lowercase.txt -u [http://\<domain>/FUZZ](http://37.18.9.109/FUZZ) -c -r -mc 200

## Перебор директорий

```
ffuf -w .../tools/pentest/wordlists/dirsearch/db/dicc.txt -o ffuf_result.json -r -D -u https://example.com/FUZZ -e js,php,asp,aspx,jsp > out.txt
```

## Virtual Hosts

&#x20;см в соотв разделе

## Перенаправить на Burp

```
-x http://127.0.0.1:8080/
```
