# Java

## About

Java Native Binary — нативная сериализация в Java позволяет сохранять Java-объекты в бинарный формат. Используется в том числе в RMI (Remote Method Invocation), JMX (Java Management Extension), JMS (Java Message Service).

Основная опасность использования нативной Java-сериализации состоит в том, что атакующий может отправить приложению специальным образом сформированный объект, десериализация которого приводит к выполнению произвольного кода. Классы таких объектов называются "гаджетами". На сегодняшний день исследователями уже найдено большое количество гаджетов в популярных библиотеках (таких как Apache Commons Collections, Hibernate, Spring Framework, Jdk7 и т.д.).

## Что ищем

```java
ObjectMessage.getObject()
MapMessage.getObject(), getObjectProperty()
```

## Papers

Java Deserialization cheat sheet: [https://github.com/GrrrDog/Java-Deserialization-Cheat-Sheet](https://github.com/GrrrDog/Java-Deserialization-Cheat-Sheet)

Норм статья про сериализацию: [https://cyberpolygon.com/ru/materials/insecure-deserialisation/](https://cyberpolygon.com/ru/materials/insecure-deserialisation/)

## Tools

ysoserial (Java) - тулза для создания гаджетов на десериализацию в java: [https://github.com/frohoff/ysoserial](https://github.com/frohoff/ysoserial)

JexBoss (Python) - атаки на десериализацию в Java.  [https://github.com/joaomatosf/jexboss](https://github.com/joaomatosf/jexboss)

## Mitigation

Для того, чтобы избежать атак, связанных с десериализацией гаджетов, рекомендуется проверять имя класса перед десериализацией (Lookahead deserialization pattern).

Всего существует два подхода к проверке:

1. "Белый" список имён разрешенных классов. В этом случае остальные классы блокируются для десериализации.
2. "Черный" список имён запрещенных классов-гаджетов.

Рекомендуется использовать "белый" список, так как защита по "черному" списку может быть нейтрализована атакующим с помощью использования "новых" гаджетов.

Реализовать такую защиту можно переопределив метод "resolveClass()" в классе ObjectInputStream:

```java
...
public class MyObjectInputStream extends ObjectInputStream {
...
    @Override
    protected Class<?> resolveClass(ObjectStreamClass desc) throws IOException, ClassNotFoundException {
        if (!desc.getName().equals("MyObjectInputStream.Example")) {
            throw new InvalidClassException("Unauthorized deserialization attempt", desc.getName());
        }
        return super.resolveClass(desc);
    }
}
```

Пример использования Lookahead-паттерна для десериализации:

```java
ByteArrayInputStream bais = new ByteArrayInputStream(buffer);
ObjectInputStream ois = new MyObjectInputStream(bais);
Example obj = (Example) ois.readObject();
ois.close();
bais.close();
```

Данный подход с применением "белых" и "черных" списков реализован в библиотеке [SerialKiller](https://github.com/ikkisoft/SerialKiller).
