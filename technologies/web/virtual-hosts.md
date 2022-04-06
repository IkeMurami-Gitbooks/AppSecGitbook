# Virtual Hosts

За одним ip, можеет быть несколько виртуальных хостов (считай домены на одном ip). их надо брутить. Они могут разводиться внуутри на разные бэки за фронтом, а еще может быть такое, как в nginx  c upstream. &#x20;

Пример команды для брута хостов

```
ffuf -w ip.txt:IP -w domains.txt:VIRTUAL -o ffuf_result.json -r  -u https://IP/ -H "Host: VIRTUAL" -x http://127.0.0.1:8080/ 
```

\-x -> для лога в бурп (там удобнее фильтровать потом через Logger++)

Примеры virtual hosts:

```
localhost
secure
infra
pre
prerelease
dev
int
```

Kubernetes:

```
kubernetes.default.svc
    https://kubernetes.default.svc/info
    https://kubernetes.default.svc/livez?verbose
```
