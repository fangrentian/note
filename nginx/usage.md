# 配置访问日志


## 使用proxy_pass

在 `http` 下定义访问日志的格式, 如示例中的 `custom`, 在相应的 `location` 下配置 `proxy_pass` 和 `access_log`;

## 使用 `map` 定义日期变量 `$logdate`，日志按日分割时可使用
```shell
map $time_iso8601 $logdate {
    '~^(?<ymd>\\d{4}-\\d{2}-\\d{2})' $ymd;
    default                       'date-not-found';
}
```

## 使用 `geo` 定义ip白名单
```shell
geo $remote_addr $geo {
    proxy 119.181.143.126;
    proxy 61.173.27.199;
    default 1; 
    127.0.0.1 0; 
    119.181.143.126 0; 
    61.173.27.199 0; 
    58.220.95.43 0; 
}
```
约定 `$geo` 为0时放行， 为1时拦截

## 完整配置示例
```conf

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;
     map $time_iso8601 $logdate {
        '~^(?<ymd>\\d{4}-\\d{2}-\\d{2})' $ymd;
        default                       'date-not-found';
    }
    
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    log_format  custom escape=json  '客户端地址: $remote_addr \n '
                                    '跳转来源: $http_referer \n '
                                    '用户终端代理: $http_user_agent \n '
                                    '请求cookie: $http_cookie \n '
                                    '请求方法,URI及协议: $request \n '
                                    '请求地址: $http_host \n '
                                    '请求时间: $time_local \n '
                                    '请求方法: $request_method \n '
                                    '请求路径: $uri \n '
                                    '请求参数: $query_string \n '
                                    '请求体: $request_body \n';

    # access_log  logs/access-$logdate.log  custom;
    # open_log_file_cache max=10;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    geo $remote_addr $geo {
	    proxy 119.181.143.126;
        proxy 61.173.27.199;
        default 1; 
        127.0.0.1 0; 
        119.181.143.126 0; 
        61.173.27.199 0; 
        58.220.95.43 0; 
    }

    server {
        listen       10001;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access-$logdate.log  main;
        
        if ( $geo = 1 ) {
            return 403;
        }

        location / {
            root   html;
            index  index.html index.htm;

            proxy_pass   https://miao.baidu.com/abdr;
            #   指定允许跨域的方法，*代表所有
            add_header Access-Control-Allow-Methods *;

            #   预检命令的缓存，如果不缓存每次会发送两次请求
            add_header Access-Control-Max-Age 3600;
            #   带cookie请求需要加上这个字段，并设置为true
            add_header Access-Control-Allow-Credentials true;

            #   表示允许这个域跨域调用（客户端发送请求的域名和端口） 
            #   $http_origin动态获取请求客户端请求的域   不用*的原因是带cookie的请求不支持*号
            add_header Access-Control-Allow-Origin $http_origin;

            #   表示请求头的字段 动态获取
            add_header Access-Control-Allow-Headers $http_access_control_request_headers;

            #   OPTIONS预检命令，预检命令通过时才发送请求
            #   检查请求的类型是不是预检命令
            if ($request_method = OPTIONS){
                return 200;
            }
            access_log  logs/access.log  custom;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
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


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

```
