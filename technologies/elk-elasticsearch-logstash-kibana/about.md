# About

[https://misteam.medium.com/%D0%BF%D0%B5%D0%BD%D1%82%D0%B5%D1%81%D1%82-elk-250eb4806fab](https://misteam.medium.com/%D0%BF%D0%B5%D0%BD%D1%82%D0%B5%D1%81%D1%82-elk-250eb4806fab)

ELK — один из самых популярных стеков технологий. В очень многих компаниях используется связка Elasticsearch-Kibana-Logstash, в которой порой хранится очень интересная и важная информация (чаще всего это логи с различных систем). И очень часто при RedTeam эти логи могут помочь — доменные имена, ip-адреса, имена пользователей, какая-то информация, которая может косвенно помочь — все это мы встречали в своей практике в ELK разных компаний.

Чаще всего пентест ELK начинается с того, что найдена кибана и из нее надо получить данные, а в идеале — добраться до всех данных в elasticsearch. Если развернута elk — значит есть необходимость обрабатывать большой объем данных.
