### Задание 3*
- Настройте связку HAProxy + Nginx как было показано на лекции.
- Настройте Nginx так, чтобы файлы .jpg выдавались самим Nginx (предварительно разместите несколько тестовых картинок в директории /var/www/), а остальные запросы переадресовывались на HAProxy, который в свою очередь переадресовывал их на два Simple Python server.
- На проверку направьте конфигурационные файлы nginx, HAProxy, скриншоты с запросами jpg картинок и других файлов на Simple Python Server, демонстрирующие корректную настройку.

### Решение 3*

Файл example-thhp.conf
```yml
server {
   listen	80;

   server_name	example-http.com;
   
   access_log	/var/log/nginx/example-http.com-acess.log;
   error_log	/var/log/nginx/example-http.com-error.log;

   location ~* \.(jpg|jpeg)$ {
      root         /var/www;
      access_log   off;
      expires      3d;
   }

   location / {
      proxy_pass	http://localhost:1325;
   }
}
```

Часть файла haproxy.cfg
```yml
listen stats
        bind                    :888
        mode                    http
        stats                   enable
        stats uri               /stats
        stats refresh           5s
        stats realm             Haproxy\ Statistics

listen web_tcp
        bind :1325
        server s1 127.0.0.1:8888 check inter 3s
        server s2 127.0.0.1:9999 check inter 3s
```

![Задание 3*](https://github.com/karelinsf/haproxy_nginx/blob/main/HAPROXY-NGINX/screen/haproxy-nginx.png)

![Задание 3*](https://github.com/karelinsf/haproxy_nginx/blob/main/HAPROXY-NGINX/screen/file-jpg.png)