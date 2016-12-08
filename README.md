# php-fpm

此镜像为基于php:5.6.28-fpm

主要是安装了一些常用的拓展，如libpng、libjpeg、gd、mysql，还安装了xdebug，用于调试

可在linux/win10上使用

启动命令：
```
docker run -it --rm --name php -p 9000:9000 -v d:\work\www:/www -v d:\work\www\docker\php.ini:/usr/local/etc/php/php.ini 362228416/php-fpm
```

nginx 配置
```
server {
        listen       7000;
        server_name  localhost;
        location / {
            root .; 这里看自己的配置
            index  index.php index.html index.htm;
        }
        
        location ~ \.php$ {
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header Host $http_host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  /www/$fastcgi_script_name;
            fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
            include        fastcgi_params;
        }
}
```

php.ini
```
date.timezone = Asia/Shanghai
display_errors = On
short_open_tag = On

[xdebug]
zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20131226/xdebug.so
xdebug.remote_enable = on
xdebug.remote_connect_back = on
xdebug.remote_handler = dbgp
xdebug.remote_port = 9001

```

启动nginx打开
http://localhost:7000/ 即可

调试的话在idea里面需要装php插件，具体怎么配，在网上能找到

此镜像已上传到 https://hub.docker.com/

阿里云有加速镜像 
docker pull registry.cn-hangzhou.aliyuncs.com/362228416/php-fpm

阿里云docker仓库
https://dev.aliyun.com/search.html
