# Passwords

Passwords Frances: [https://github.com/tarraschk/richelieu](https://github.com/tarraschk/richelieu)

SecLists: [https://github.com/danielmiessler/SecLists/tree/master/Passwords](https://github.com/danielmiessler/SecLists/tree/master/Passwords)

rockyou&#x20;

Более современный интернациональлный RockYou: [https://github.com/FlameOfIgnis/Pwdb-Public](https://github.com/FlameOfIgnis/Pwdb-Public)

В YAWR (в том числе и не плохие русские пароли) — \*/passwords/

Так же от @empty\_jack словарик на простые популярные пароли админов: [https://github.com/empty-jack/YAWR/blob/master/passwords/realyBest.txt](https://github.com/empty-jack/YAWR/blob/master/passwords/realyBest.txt)

Генератор паролей из комбинаций геометрических комбинаций клавиш на клавиатуре (Keyboard Walk Passwords; например, 1qazxsw2) [https://github.com/hashcat/kwprocessor](https://github.com/hashcat/kwprocessor)\
запускается вот так\
`./kwp basechars/full.base keymaps/en.keymap routes/2-to-10-max-3-direction-changes.route`\
Классная штука, но не совсем то что надо. Он генерирует неразрывные шаблоны. То есть он сгенерит 1qazxsw2, а 1qaz2wsx он не сгенерит. На его основе можно делать гигантские словари для хешката. большая часть слов в этих словарях будет бесполезной, но что-то может стрельнуть.\
От @sorokinpf словарь на десятки тысяч различных геометрических паролей: [https://github.com/sorokinpf/cth\_wordlists/tree/master/passwords/keyboard](https://github.com/sorokinpf/cth\_wordlists/tree/master/passwords/keyboard)

russian [https://github.com/sharsi1/russkiwlst](https://github.com/sharsi1/russkiwlst)

