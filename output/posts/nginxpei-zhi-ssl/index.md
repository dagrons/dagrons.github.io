<!--
.. title: nginx配置ssl
.. slug: nginxpei-zhi-ssl
.. date: 2021-09-21 16:26:07 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

## 问题分析
在很多时候, 我们需要实现加密通信, 例如对于mal应用而言, 由于要上传恶意软件, 势必会被防火墙拦截, 因此需要配置ssl, 才能绕过这些限制

## 生成自签名证书
```bash
openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
```

## nginx配置ssl
```
sudo mkdir /etc/nginx/cert && cp *.pem /etc/nginx/cert
```
/etc/nginx/sites-enabled/mal
```
 server {
          listen 5000 ssl;

          ssl_certificate  /etc/nginx/cert/certificate.pem;
          ssl_certificate_key  /etc/nginx/cert/key.pem;
  
          location ~ ^/api/v2/ {
                   include proxy_params;
                  proxy_pass http://unix:/home/dell/mal/mal.sock;
         }
 
         location ~ ^/ {
                  root /var/www/html/mal_ui;
                  index index.html;
         }
}
```
检查配置
```
sudo nginx -t
```
配置生效
```
sudo nginx -s reload
```