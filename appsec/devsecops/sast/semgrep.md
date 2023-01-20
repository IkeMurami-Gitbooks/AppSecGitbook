# Semgrep

**Semgrep**: сканер исходного кода. Есть куча модулей (1к+), создаваемых сообществом. Не раз уже видел ссылку на эту тулзу: [https://github.com/returntocorp/semgrep](https://github.com/returntocorp/semgrep)

Использование

```
semgrep --config "p/golang" -l go -f /path/to/custom/rules1 -f /path/to/custom/rules2 [/path/to/src]
```

Посмотреть все rulesets: [https://semgrep.dev/explore](https://semgrep.dev/explore)\
Через `--config` указываем ruleset (можно несколько). Через `-l` можно ограничить язык(и).

Устанавливается semgrep через pip: `python3 -m pip install semgrep`.

Semgrep + Ghidra = Search Binary Weaks: [https://security.humanativaspa.it/automating-binary-vulnerability-discovery-with-ghidra-and-semgrep/](https://security.humanativaspa.it/automating-binary-vulnerability-discovery-with-ghidra-and-semgrep/)

## Rules

Some rules: [https://github.com/elttam/semgrep-rules](https://github.com/elttam/semgrep-rules)

Rules for PHP security assessment: [https://security.humanativaspa.it/semgrep-rules-for-php-security-assessment/](https://security.humanativaspa.it/semgrep-rules-for-php-security-assessment/)
