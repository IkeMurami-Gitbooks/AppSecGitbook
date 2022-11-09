# JSON

Статья про JSON deserialisation (а внутри есть ссылки и на другие исследования)\
[us-17-Munoz-Friday-The-13th-JSON-Attacks-wp.pdf](https://trello-attachments.s3.amazonaws.com/5b5ad6aad2b76d04c3baead5/5d31d4d39bfc922e7c9ee78d/a906e971b35354cc5456ad49bb770787/us-17-Munoz-Friday-The-13th-JSON-Attacks-wp.pdf)

## Java Jackson (JSON)

Jackson - Java библиотека для работы с данными в формате JSON. При десериализации Jackson использует конструктор по-умолчанию и setter-методы.

При использовании настроек библиотеки по-умолчанию Jackson проверяет соответствие между ожидаемым и реальным классом. Таким образом, атакующий не может десериализовать объект произвольного класса, и ограничен только заданным классом.

Рекомендуется использовать Jackson следующим образом:

```java
ObjectMapper objectMapper = new ObjectMapper();
User webUser = objectMapper.readValue(webUserJackson, User.class);
```

Полиморфная десериализация без указания класса (использование Object.class) небезопасна. Атакующий может десериализовать произвольный класс и [добиться выполнения произвольного кода](https://adamcaudill.com/2017/10/04/exploiting-jackson-rce-cve-2017-7525/).

```java
ObjectMapper objectMapper = new ObjectMapper();
objectMapper.enableDefaultTyping();
User webUser = (User) objectMapper.readValue(webUserJackson, Object.class);
```
