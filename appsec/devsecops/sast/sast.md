# SAST List

## SAST TOP

Checkmarx \
Fortify (for Java) — запускать лучше через Audit Workbench. (@ramon93i7 рекомендует)\
Coverity

## Mobile SAST

mobsfscan = semgrep + libsast: [https://github.com/MobSF/mobsfscan](https://github.com/MobSF/mobsfscan)

## Others

Code Pulse (C#/Scala/..) - проект OWASP для анализа приложений (C#/Java) на предмет безопасного кода (Возможно, в процессе выполнения. Т.е. это DAST). site:[ https://code-pulse.com](https://code-pulse.com). git: [https://github.com/codedx/codepulse](https://github.com/codedx/codepulse)

MS Application Scanner - шляпа полнейшая

Solar AppScreener (до 3.0 — Solar inCode — сканер, который разрабатывает SoftLine Group) — C++ — сканер от РосТелекома. SAST/DAST инструмент.

PVS-Studio\
SonarCube - хорош для codestyle, Java/php, недорогой, используется в pipeline в devops. Работает примерно следующим образом. Есть сервер, есть клиент, который запускаешь у себя локально, он анализирует твой код (лучше всего чтобы был SCM настроен, иначе не взлетит) и отправляет все на сервак, где проводится анализ.\
AppScan ?

По разным языкам таблица: [https://docs.gitlab.com/ee/user/application\_security/sast/](https://docs.gitlab.com/ee/user/application\_security/sast/)

От OWASP список [https://owasp.org/www-community/Source\_Code\_Analysis\_Tools](https://owasp.org/www-community/Source\_Code\_Analysis\_Tools)

JS: **retire.js** - обнаружение использования уязвимых библиотек JS. Распространяется как расширение для chrome.

Brakeman — for RubyOnRails free scanner — [https://brakemanscanner.org/](https://brakemanscanner.org/)

Python (от Facebook): Pyre & Pysa [https://pyre-check.org/](https://pyre-check.org/)

Android & Java app (от Facebook): [https://mariana-tren.ch/](https://mariana-tren.ch/)

Android, Java, C, C++, ObjC (от Facebook): [https://fbinfer.com/](https://fbinfer.com/)

Code Verify (от Facebook) — проверяет целостность и какие-то еще ошибки загруженного в браузере веб-приложения [https://github.com/facebookincubator/meta-code-verify/](https://github.com/facebookincubator/meta-code-verify/)

infer (от Facebook) — еще какой-то анализатор [https://github.com/facebook/infer](https://github.com/facebook/infer)

wiggle — ищет интересную функциональность в C/C++ проектах (от Google Project Zero): [https://github.com/googleprojectzero/weggli](https://github.com/googleprojectzero/weggli)&#x20;

### semgrep

**Semgrep**: сканер исходного кода. Есть куча модулей (1к+), создаваемых сообществом. Не раз уже видел ссылку на эту тулзу: [https://github.com/returntocorp/semgrep](https://github.com/returntocorp/semgrep)

Использование

```
semgrep --config "p/golang" -l go -f /path/to/custom/rules1 -f /path/to/custom/rules2 [/path/to/src]
```

Посмотреть все rulesets: [https://semgrep.dev/explore](https://semgrep.dev/explore)\
Через `--config` указываем ruleset (можно несколько). Через `-l` можно ограничить язык(и).

Устанавливается semgrep через pip: `python3 -m pip install semgrep`.

Semgrep + Ghidra = Search Binary Weaks: [https://security.humanativaspa.it/automating-binary-vulnerability-discovery-with-ghidra-and-semgrep/](https://security.humanativaspa.it/automating-binary-vulnerability-discovery-with-ghidra-and-semgrep/)




