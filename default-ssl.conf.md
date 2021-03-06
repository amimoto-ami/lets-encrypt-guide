# default-ssl.conf

``` bash
server {
    listen      443 ssl http2;
    server_name _;
    root        /var/www/vhosts/i-abc123;
    index       index.html index.htm;
    charset     utf-8;

    ssl on;
    ssl_certificate     /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_ciphers AESGCM:HIGH:!aNULL:!MD5;
    ssl_session_cache   shared:SSL:10m;
    ssl_session_timeout 5m;

    access_log  /var/log/nginx/default-ssl.access.log  main;
    error_log   /var/log/nginx/default-ssl.error.log;

    include     /etc/nginx/drop;

    add_header X-Cache-Status $upstream_cache_status;
    expires $expires;

    set $mobile '';
    #include /etc/nginx/mobile-detect;

    include     /etc/nginx/wp-front;

    location ~* /(phpmyadmin|myadmin|pma) { access_log off; log_not_found off; return 404; }

    #
    # redirect server error pages to the static page /50x.html
    #
    error_page  502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```
