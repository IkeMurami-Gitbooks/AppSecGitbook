# Making Authenticated Requests

Независимо от того, какой grant type использовался, есть ли у клиента client secret или нет, у вас теперь есть OAuth 2.0 Bearer Token.

Есть два способа, как API сервера могут обработать Bearer-токены: в HTTP-заголовке `Authorization` или в теле POST-запроса.
