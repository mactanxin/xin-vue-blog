# 部署vue项目到nains on CentOS7
# 安装nginx
```
sudo yum install epel-release
yum update
yum install nginx
```
失败之后一直提示找不到包, 替换为新命令解决
```
rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
yum install nginx -y
```
## 配置ngxin
默认nginx 配置文件位置 : /etc/nginx/nginx.conf
修改文件 添加 include
```
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/default.conf;
}
```

之后修改 `/etc/nginx/conf.d/default.conf`

```
server {
    listen       4200;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/local/www/tanxin/dist;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

遇到报错: `nginx: [error] invalid PID number "" in "/var/run/nginx.pid"`

 在 nginx -t 的时候一切正常 : nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
但是 `nginx -s reload`  的时候会报错.
需要 `nginx -c /etc/nginx/nginx.conf` 之后再 `nginx -s reload`
就好了
## 部署打包文件
在本地 `build` 之后生成 `dist/` 文件夹. 然后 `scp -r dist/ root@192.168.2.221:/usr/local/www/tanxin`

之后 `nginx -s reload`就可以了.