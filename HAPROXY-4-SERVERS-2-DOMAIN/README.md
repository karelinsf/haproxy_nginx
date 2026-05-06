### Задание 4*
- Запустите 4 simple python сервера на разных портах.
- Первые два сервера будут выдавать страницу index.html вашего сайта example1.local (в файле index.html напишите example1.local)
- Вторые два сервера будут выдавать страницу index.html вашего сайта example2.local (в файле index.html напишите example2.local)
- Настройте два бэкенда HAProxy
- Настройте фронтенд HAProxy так, чтобы в зависимости от запрашиваемого сайта example1.local или example2.local запросы перенаправлялись на разные бэкенды HAProxy
- На проверку направьте конфигурационный файл HAProxy, скриншоты, демонстрирующие запросы к разным фронтендам и ответам от разных бэкендов.

### Решение 4*

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
        bind :80

        acl ACL_example1.local hdr(host) -i example1.local
        use_backend web_servers1 if ACL_example1.local

        acl ACL_example2.local hdr(host) -i example2.local
        use_backend web_servers2 if ACL_example2.local


backend web_servers1
        mode http
        balance roundrobin
        option httpchk
        http-check send meth GET uri /index.html
        server s1 127.0.0.1:8888 check
        server s2 127.0.0.1:8800 check

backend web_servers2
        mode http
        balance roundrobin
        option httpchk
        http-check send meth GET uri /index.html
        server s3 127.0.0.1:9999 check
        server s4 127.0.0.1:9900 check

```

![Задание 4*](https://github.com/karelinsf/haproxy_nginx/blob/main/HAPROXY-4-SERVERS-2-DOMAIN/screen/haproxy-4-servers-2-domain.png)

![Задание 4*](https://github.com/karelinsf/haproxy_nginx/blob/main/HAPROXY-4-SERVERS-2-DOMAIN/screen/haproxy-4-servers-2-domain-stats.png)
