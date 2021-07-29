# dgca-issuance-web-setup-guide

## 下載
由於後面的版本有`business-rule`目前沒研究到，後續也沒有要研究，所以使用`1.0.8`版本。
- `wget https://github.com/eu-digital-green-certificates/dgca-issuance-web/archive/refs/tags/1.0.8.tar.gz`
- `tar xzvf  1.0.8.tar.gz`

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
