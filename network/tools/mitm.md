# MiTM

MiTM Cheet Sheets\
[https://github.com/Sab0tag3d/MITM-cheatsheet](https://github.com/Sab0tag3d/MITM-cheatsheet)

Набор утилит для MiTM атак: [https://github.com/websploit/websploit](https://github.com/websploit/websploit)

mitm-relay\
Весь трафик, входящий на порт 1337, перекидываем на [google.com](http://google.com):433, при этом копия отправляется на burp localhost:8080 с ключом сервера (ключ burp) [server.pem/server.key](http://server.pem/server.key)

```
python mitm_relay.py -r tcp:1337:google.com:443 -p localhost:8080 -c server.pem -k server.key
```

mitm6: [https://github.com/fox-it/mitm6](https://github.com/fox-it/mitm6)
