# Impacket

Impacket - коллекция Python классов для работы с сетевыми протоколами на низком уровне\
[https://github.com/SecureAuthCorp/impacket](https://github.com/SecureAuthCorp/impacket)

```
../impacket/examples# python3 mssqlclient.py sa:"password"

...
SQL> xp_cmdshell whoami

...
```

ntlmrelayx — часть Impacket'а. Его задача проста — когда к нему приходит пакет, он его перенаправляет на указанный хост (например, для SMB Relay атаки)

## Установка

```
$ git clone https://github.com/SecureAuthCorp/impacket
$ cd impacket
$ python3 -m venv .venv
$ source .venv/bin/activate
$ pip install -r requirements.txt
$ pip install .
$ python examples/ntlmrelayx.py -h
$ deactivate
```
