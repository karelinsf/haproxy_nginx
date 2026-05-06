# Домашнее задание к занятию 2 «Кластеризация и балансировка нагрузки» - Карелин С.Ф.

### Задание 1
- Запустите два simple python сервера на своей виртуальной машине на разных портах
- Установите и настройте HAProxy, воспользуйтесь материалами к лекции
- Настройте балансировку Round-robin на 4 уровне.
- На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

### Решение 1

Часть файла haproxy.cfg
```yml
listen stats
        bind                    :888
        mode                    http
        stats                   enable
        stats uri               /stats
        stats refresh           5s
        stats realm             Haproxy\ Statistics

frontend example
        mode http
        bind :8088

listen web_tcp
        bind :1325
        server s1 127.0.0.1:8888 check inter 3s
        server s2 127.0.0.1:9999 check inter 3s
```

![Задание 1](https://github.com/karelinsf/haproxy_nginx/blob/main/HAPROXY/screen/round-robin.png)

![Задание 1](https://github.com/karelinsf/haproxy_nginx/blob/main/HAPROXY/screen/haproxy-L4.png)