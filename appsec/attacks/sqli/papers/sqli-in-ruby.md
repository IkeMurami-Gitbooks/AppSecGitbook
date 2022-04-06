# SQLi in Ruby

Только посмотрите, как круто в коде `RubyOnRails` выглядит `SQLi` в ORDER BY:\
`Client.order(:first_name)`\
Т.е. Active Record Query Interface после такого вызова построит следующий запрос:\
`'SELECT * FROM Clients ORDER BY #{:first_name}'` , где `#{:first_name}` - это user input.\
Метод `.order()` не фильтрует и не экранирует получаемые данные. А значит, в таком случае мы легко можем раскрутить `Blind SQLi` c подзапросом вроде:\
`ORDER BY 1,(SELECT 1 FROM SLEEP(5))`-- , где дальше можно крутить Boolean-based, Time-based и пр.\
P.S. Почему это круто?!\
Потому что разработчики, когда пишет код, даже не подозревает, что вызов вроде `Client.order(:first_name)` приведет к `SQLi`. Тут ничего не режет глаз, нет конкатенации, нет опасных функций, казалось бы и пр.
