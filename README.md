# dgca-issuance-web-setup-guide

## 下載
由於後面的版本有`business-rule`目前沒研究到，後續也沒有要研究，所以使用`1.0.8`版本。
- `wget https://github.com/eu-digital-green-certificates/dgca-issuance-web/archive/refs/tags/1.0.8.tar.gz`
- `tar xzvf  1.0.8.tar.gz`

## 設定nginx
你可以加入自己要的帳號密碼或是把驗證給關了。
### htpasswd (加入自己的帳號密碼)
- 進入`nginx`資料夾
- 輸入以下指令
```
htpasswd -b .htpasswd $username $password
```
> $username請輸入你的帳號 
> $password請輸入你的密碼

### 移除auth (移除網頁登入)
- 修改`nginx/default.conf/template`
- 註解掉`auth_basic`以及`auth_basic_user_file`
```
server {
    listen ${SERVER_PORT};
    server_tokens off;
    location / {
        #auth_basic "Secured Site";
        #auth_basic_user_file /etc/nginx/.htpasswd;
        root /usr/share/nginx/html;
        index unresolvable-file-html.html;
        try_files $uri @index;
    }
    location @index {
        root /usr/share/nginx/html;
        add_header Cache-Control no-cache;
        expires 0;
        try_files /index.html =404;
    }
    location /dgca-issuance-service/ {
        proxy_ssl_server_name on;
        proxy_pass ${DGCA_ISSUANCE_SERVICE_URL}/;
    }
    location /dgca-businessrule-service/ {
        proxy_ssl_server_name on;
        proxy_pass ${DGCA_BUSINESSRULE_SERVICE_URL}/;
    }
}
```


## docker-compose.yaml
於[dgca-issuance-service的docker-compose.yaml](https://github.com/DGC-TW-POC/dgca-issuance-service-setup-guide/blob/main/README.md#docker-compose)加入
```
version: '3'

services:

...

  dgc-issuance-web:
    build: ./dgca-issuace-web #./dgca-issuance-web-1.0.8
    image: dgca-issuance-web/dgca-issuance-web
    container_name: dgc-issuance-web
    hostname: dgc-issuance-web
    ports:
    - 8088:80
    environment:
    - SERVER_PORT=80
    - DGCA_ISSUANCE_SERVICE_URL=http://dgc-issuance-service:8080
    networks:
      backend:
      
...

```

- `sudo docker-compose up dgc-issuance-web`
