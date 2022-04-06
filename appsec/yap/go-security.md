# Go Security

## Golang Security

В силу того, что все больше компаний начинают переписывать свои сервисы на golang, решил сделать подборку по аспектам безопасности этого языка.

[Awesome golang security](https://github.com/guardrailsio/awesome-golang-security) - подборка лучших практик, включая библиотеки, фреймворки для харденинга, статьи

[OWASP Go-SCP](https://github.com/OWASP/Go-SCP) - лучшие практики по написанию безопасного кода на Go от OWASP

[Go-ing for an Evening Stroll: Golang Beasts & Where to Find Them](https://www.youtube.com/watch?v=a1qrjtrmOj0) - обсуждение 4х распространенных уязвимостей в Golang. [Репо](https://github.com/lojikil/kyoto-go-nihilism) с расширенной версией доклада + слайды

Статические анализаторы: [gosec](https://github.com/securego/gosec), [golangci-lint](https://github.com/golangci/golangci-lint), [safesql](https://github.com/stripe/safesql)

gosec включен в semgrep: `semgrep --config "p/gosec"`

[Not-going -anywhere](https://github.com/trailofbits/not-going-anywhere/) - набор уязвимых программ на Golang для выявления распространенных уязвимостей.

[OnEdge](https://github.com/trailofbits/on-edge) - библиотека для обнаружения неправильного использования паттернов Defer, Panic, Recover.
