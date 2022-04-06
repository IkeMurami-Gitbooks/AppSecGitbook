# Session Fixation

В чем примерный кейс:&#x20;

Атакующий заходит на сайт и получает аутентификационную куку (например, PHPSESSIONID). С помощью cookie poisoning жертве встривается аутентификационная кука атакующего. Далее жертва авторизуется на сайте, и сайт, вместо того, чтобы выдать новую куку, превращает в авторизационную - старую. Далее злоумышленник может получать по этой куке всю информацию.

Example: [https://medium.com/@novan.rmd/take-control-your-victim-account-using-session-fixation-db27841feb91](https://medium.com/@novan.rmd/take-control-your-victim-account-using-session-fixation-db27841feb91)

Mitigation: кука всегда должна выдаваться новая при авторизации и других подобных действиях
