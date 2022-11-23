# Virtual Hosts

За одним ip, можеет быть несколько виртуальных хостов (считай домены на одном ip). их надо брутить. Они могут разводиться внуутри на разные бэки за фронтом, а еще может быть такое, как в nginx  c upstream. &#x20;

Пример команды для брута хостов

```
ffuf -w ip.txt:IP -w domains.txt:VIRTUAL -o ffuf_result.json -r  -u https://IP/ -H "Host: VIRTUAL" -x http://127.0.0.1:8080/ 
```

\-x -> для лога в бурп (там удобнее фильтровать потом через Logger++)

Можем использовать без бурпа. Тогда нам надо самим будет распарсить результат ffuf:

```python
# Ищем виртуальные хосты
# ffuf -w web.ip.list:IP -w domain.ffuf.list:VIRTUAL -o ffuf_result.json -r -u https://IP/ -H Host: VIRTUAL

from pathlib import Path
import json


def introspect(obj):
    for key in obj:
        print(f'{key}: {type(obj[key])}')


with Path('D:\\MyProjects\\BugBounty\\scope\\ffuf_result.json').open(mode='r') as inp_stream:
    data = inp_stream.read()
    ffuf_json_result = json.loads(data)
    print(f'commandline: {ffuf_json_result["commandline"]}')
    print(f'time: {ffuf_json_result["time"]}')
    
    # introspect(ffuf_json_result)

    result = dict()

    for ffuf_record in ffuf_json_result['results']:
        # print(ffuf_record)
        
        IP = ffuf_record['input']['IP']
        VIRTUAL = ffuf_record['input']['VIRTUAL']
        status      = ffuf_record['status']
        length      = ffuf_record['length']
        words       = ffuf_record['words']
        lines       = ffuf_record['lines']
        duration    = ffuf_record['duration']

        status = str(status)
        length = str(length)
        words = str(words)
        lines = str(lines)
        duration = str(duration)

        if IP not in result:
            result[IP] = list()

        result[IP].append(', '.join([VIRTUAL, status, length, words, lines, duration]))
        # introspect(ffuf_record)
    
    directory = Path('D:\\MyProjects\\BugBounty\\scope\\ffuf_virtual')
    directory.mkdir(exist_ok=True)
    
    for IP in result:
        with Path(directory, IP).open(mode='w') as out_stream:
            out_stream.write('\n'.join(result[IP]))

```

И далее, чистим ffuf\_cleaner'ом

Примеры virtual hosts:

```
localhost
secure
infra
pre
prerelease
dev
int
kubernetes.default.svc
169.254.169.254
127.0.0.1
```

Kubernetes:

```
kubernetes.default.svc
    https://kubernetes.default.svc/info
    https://kubernetes.default.svc/livez?verbose
```
