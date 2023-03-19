# Dependency Confusion Attack

Атака на цепочку поставок. Суть в том, что не используется контроль репозитория (должен быть зареган соответствующий покет в публичном registry) и контроль целостности пакета (хеш). Таким образом, мы можем зарегать пакет в публичном репозитории, и тот, кто его импортирует, исполнит наш код.

Оригинальный ресерч (скорее подсветил о проблеме, и как он ее раскручивал): [https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610)

Ресерч — как сделать нагрузку и подтвердить уязвимость: [https://dhiyaneshgeek.github.io/web/security/2021/09/04/dependency-confusion/](https://dhiyaneshgeek.github.io/web/security/2021/09/04/dependency-confusion/)
