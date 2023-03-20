# Intro

Semgrep — статический анализатор исходного кода.

Документация: [https://semgrep.dev/docs/](https://semgrep.dev/docs/)

Link: [https://github.com/returntocorp/semgrep](https://github.com/returntocorp/semgrep)

Экосистема Semgrep включает в себя:

* [Semgrep OSS Engine](https://semgrep.dev/docs/getting-started/) — ядро анализатора с открытым кодом
* [Semgrep Cloud Platform (SCP)](https://semgrep.dev/docs/semgrep-app/getting-started-with-semgrep-app/) — поднимают SAST/SCA в облаке с возможностью включения в CI/CD pipelines. Это уже продукт, который распространяется за деньги (есть бесплатная квота)
* [Semgrep Code](https://semgrep.dev/products/semgrep-code) — продукт — доп пакет правил + расширенные возможности ядра (анализ над несколькими файлами, во free-версии анализ над несколькими файлами не производится).
* [Semgrep Supply Chain (SSC)](https://semgrep.dev/products/semgrep-supply-chain) — SCA инструмент от Semgrep, за деньги.

Так же:

* [Semgrep Playground](https://semgrep.dev/editor) — платформа для проверки и шеринга правил
* [Semgrep Registry](https://semgrep.dev/explore) — более 2к правил от сообщества для поиска уязвимостей, неточностей, ошибок в зависимостях&#x20;

Поддерживает более 30 языков: [https://semgrep.dev/docs/#language-support](https://semgrep.dev/docs/#language-support).

Среди них:

* Go, Java, TS/TSX/JS/JSX
* Kotlin, Rust
* C/C++, Dart, Dockerfile, HTML, Solidity, Swift
