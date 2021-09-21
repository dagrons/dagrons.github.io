<!--
.. title: gunicorn + nginx 实现 flask + react 应用部署
.. slug: gunicorn-+-nginx-shi-xian-flask-+-react-ying-yong-bu-shu
.. date: 2021-09-21 14:05:16 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->


## 问题分析
- 用gunicorn作为flask应用的生产环境服务器
- 通过nginx实现flask和react的代理连接

## wsgi入口文件 + axios配置
/home/dell/mal/runserver.py
```
from app import make_app

app = make_app()

if __name__ == "__main__":
    app.run()
```
有了上面的wsgi入口文件, 我们就可以通过gunicorn启动服务
```bash
gunicorn --bind 0.0.0.0:6000 runserver:app
```

同时前端对后端api的调用baseURL为"/api/v2"
axios/index.js
```
axios.defaults.baseURL = "/api/v2";
```

## gunicorn自启动脚本
/etc/systemd/system/mal.service
```
[Unit]
Description=malware analysis service backend
After=network.target

[Service]
User=dell
Group=www-data
WorkingDirectory=/home/dell/mal
Environment="PATH=/home/dell/Envs/mal/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
ExecStart=/home/dell/Envs/mal/bin/gunicorn --workers 3 --bind unix:mal.sock -m 007 runserver:app

[Install]
WantedBy=multi-user.target
```
然后
```
systemctl daemon-reload
systemctl start mal 
systemctl enable mal # 开机自启动
```

## react build


生成react部署文件
```bash
yarn build 
```

将部署文件拷贝到/var/www/html/mal_ui下
```bash
sudo cp -r build/* /var/www/html/mal_ui
```

## nginx配置代理
将前端代理到 ^/

将后端api代理到 ^/api/v2/

/etc/nginx/sites-available/mal
```
server {
       listen 5000;

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

然后enable该site
```bash
sudo ln -s /etc/nginx/sites-available/mal /etc/nginx/sites-enabled/
```

检查配置有无语法错误
```bash
sudo nginx -t 
```

使配置生效
```
sudo nginx -s reload
```

