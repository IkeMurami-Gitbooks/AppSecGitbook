# Definitions

**Service provider** (поставщик услуг) — агент между пользователями и другими компаниями (банки, производители товаров и услуг). Он обрабатывает (хранение, обработка и передача данных карт) данные платежных карт. Услуга Payment Gateway сюда попадает. Сервис провайдеры продают чужие ресурсы и услуги у себя

**Merchant** — принимает от пользователей данные карт за оплату своих услуг или товаров (через МП или сайт) и передает данные в Payment Service Provider. Мерчанты продают свои услуги и продукцию

**Acquiring bank** (банк-эквайер) — банк, через который идут платежи

**Issuing bank** (банк-эмитент) — банк, который выпустил карту

**Payment System** — связывает банк-эквайер и банк-эмитент, чтобы они не ходили друг к другу напрямую

**Payment Service Provider** (Payment Gateway, Платежный шлюз) — связывает Merchant'ов (МП и сайты) и банк-эквайер. Это программный модуль

**Корреспондентские счета в банков** — счета одного банка в другом для взаиморасчетов.

**NFC токены** — идентификаторы, которые нам выдает (кто? Payment Service Provider? Service provider?) в обмен на наши карточные данные. Под PCI DSS. СБП-токены — данные для платежей по СБП (не под PCI DSS)

**Реест транзакций** (платежный реестр, эквайринговый реестр) — отчет Банка по проведенным Банком транзакциям, включая (но не ограничиваясь) платежи, выплаты, возвраты ...

**MCC-код** — четырёхзначный номер, классифицирующий вид деятельности торгово-сервисной точки при операции оплаты по банковским картам или через QR СБП. Справочник — https://mcc-codes.ru/

**Корзина** — информация о платеже и его статус

**Клиринг** — процесс списания средств со счета для двустадийного платежа (для одностадийного не применимо)

**ДБО** — дистанционное банковское обслуживание. Сюда входят:

* ATM
* Call Center
* SMS Banking
* Online Banking
  * Web Banking
  * Mobile Banking
