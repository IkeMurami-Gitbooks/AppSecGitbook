# Business Logic Vuln

## Description

Business logic vulnerabilities are ways of using the legitimate processing flow of an application in a way that results in a negative consequence to the organization.

## Examples

Излишнее доверие пользовательскому вводу

Арифиметика: отрицательные, дробные значения, формулы, переполнения int/float/double..

Строки: переполнение ограничения, их обрезание и тп Например: \<long\_string>@target.email.attacker.server ->  \<long\_string>@target.email

Ослабление ограничений в процессе обработки данных (сначала все строго, но на каких-то этапах что-то становится допустимым)

Наличие или отсутствие каких-то параметров в запросе так жее может влиять на поток выполнения программы: например, при смене пароля если не указать старый пароль — что будет?

Разные манипуляции со скидками, подарочными картами и другими подобными инструментами
