
## 在主机中创建所需目录

```code
mkdir -p /apps/nginx/log
mkdir -p /apps/nginx/config
mkdir -p /apps/www/default.me
```

## 在主机中创建配置所需文件

```code
cp ./files/config/default.me.conf /apps/nginx/config
cp ./files/html/* /apps/www/default.me
```

## 启动

```code
docker compose up [-d]
```

## 多主机
- 在docker-compose.yaml中添加nginx服务的volumes

```code
      - /apps/www/www1.docker.me:/apps/www/www1.docker.me:ro,bind
```
- 在docker-compose.yaml中添加php服务的volumes

```code
      - /apps/www/www1.docker.me:/apps/www/www1.docker.me
```
- 在/apps/nginx/config目录添加nginx的虚拟主机配置

```code
server {
    listen       80;
    server_name  www1.docker.me;

    #access_log  /var/log/nginx/host.access.log  main;
    root   /apps/www/www1.docker.me;

    location / {
        #root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        #root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    location ~ \.php$ {
        #root           html;
        fastcgi_pass   lamp-php72:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    location ~ /\.ht {
        deny  all;
    }
}

```
