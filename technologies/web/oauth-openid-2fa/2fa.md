# 2FA

## Papers

2FA Bypass [https://medium.com/@surendirans7777/2fa-bypass-techniques-32ec135fb7fe](https://medium.com/@surendirans7777/2fa-bypass-techniques-32ec135fb7fe)

Какая-то статья про 2FA (как, по мнению автора должно быть)\
[https://blog.trailofbits.com/2019/06/20/getting-2fa-right-in-2019/](https://blog.trailofbits.com/2019/06/20/getting-2fa-right-in-2019/)

## Атаки на 2FA

1 Bypass 2FA через управление сессией\
Этот подход заключается в обходе 2FA через сброс пароля. Во многих веб приложениях после сброса пароля пользователь автоматически логинется в приложении (2фа не реализовано системой в этом случае)\
Порядок действий следующий:\
Меняем пароль > запрашиваем токен для сброса пароля > используем токен > логинемся в приложении\
2 Bypass 2FA via Oauth mechanism\
Здесь идет речь об интеграции авторизации через 3th party аккаунты (гугл, фб, и тп)\
При такой авторизации иногда 2FA не запрашивается. => если есть есть доступ к фб аккаунту - получаем доступ и к основному без 2фа\
3 Брут кода 2fa\
4 Race Condition\
Отправка нескольких значений на одном коде или что подобное (не оч понятно из описания)

![](<../../../.gitbook/assets/2020-05-22 00.57.23.jpg>)

Другой mindmap: [https://www.xmind.net/m/8Hkymg/#](https://www.xmind.net/m/8Hkymg/)

## Типы токенов (одна из классификаций) при 2FA

`Event-Based Token (HOTP)`: An OTP system generates event-based tokens on demand using a combination of a static random key value (HMAC; the H in HOTP) and a dynamic value, such as a counter (IETF, 2005). The event-based token is usually valid for a variable amount of time, but could be valid for an unlimited amount of time.\
\
`Time-Based Token (TOTP)`: An OTP system generates time-based tokens automatically every so often based on a static random key value and a dynamic time value (such as currently time of day). The time-based token is only valid for a certain amount of time, such as 30 or 60 seconds (IETF, TOTP: Time-Based One-Time Password Algorithm, 2011). TOTP is a subset of HOTP.\
\
`Challenge-Based Token (OCRA)`: An OTP system generates challenge-based tokens on demand (IETF, OCRA: OATH Challenge-Response Algorithm, 2011), using a random challenge key that is provided by the authentication server at each unique user log-in. The challenge-based token is valid for a certain amount of time such as several minutes.

## M13-Team заметки

![](<../../../.gitbook/assets/photo\_2020-10-24 15.45.47.jpeg>)

## Secure Way

Password + TOTP

FIDO2 (физ токены)

?WebAuthn
