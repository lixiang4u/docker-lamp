
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

