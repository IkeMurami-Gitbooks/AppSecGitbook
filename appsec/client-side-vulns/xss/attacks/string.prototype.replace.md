# String.prototype.replace

Нельзя допускать вставки во второй аргумент пользовательского ввода, тк это может привести к xss из-за вставки [шаблона](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/String/replace#%D0%BF%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B0\_%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%B8\_%D0%B2\_%D0%BA%D0%B0%D1%87%D0%B5%D1%81%D1%82%D0%B2%D0%B5\_%D0%B2%D1%82%D0%BE%D1%80%D0%BE%D0%B3%D0%BE\_%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%B0).

