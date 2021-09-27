<!--
.. title: supervisor实现项目级别的进程管理
.. slug: supervisorshi-xian-xiang-mu-bu-shu
.. date: 2021-09-27 12:15:40 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->


## supervisor简介

- 相比于systemd, supervisor更适合项目级别的进程管理


## 一个典型的supervisor配置

- supervisord指定了logfile等supervisord的进程环境
- unix_http_server指定了supervisorctl与supervisord的通信方式
- inet_http_server支持web界面管理进程组
- supervisorctl指定了supervisorctl相关配置

```ini
[supervisord]
logfile = ./.supervisord/supervisord.logfile
logfile_maxbytes = 50MB
loglevel = info
pidfile = ./.supervisord/supervisord.pid
umask = 022
environment = KEY1="value1",KEY2="value2"

[unix_http_server]
; unix socket for supervisorctl to communicate to 
file = /tmp/supervisor.sock
chmod = 0777
chown = dell
username = dell
password = daxiahyh

[inet_http_server]
; this enable http web gui
port = 127.0.0.1:9001
username = dell
password = daxiahyh

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface 

[supervisorctl]
serverurl = unix:///tmp/supervisor.sock
username = dell
password = daxiahyh
prompt = mysupervisor

[program:flask]
command = /home/dell/Envs/mal/bin/gunicorn --workers 4 --bind unix:mal.sock -m 777 runserver:app
process_name = %(program_name)s
numprocs = 1
directory = /home/dell/mal_v2
environment = FLASK_APP="app"
stdout_logfile = ./.supervisord/mal.logfile
redirect_stderr = true

[program:celery]
command = /home/dell/Envs/mal/bin/celery -A app.celery worker -c 4 --loglevel info
process_name = %(program_name)s
directory = /home/dell/mal_v2
stdout_logfile = ./.supervisord/celery.logfile
redirect_stderr = true

[group:mal]
programs = flask, celery
priority = 999

```

## nginx部署

注意, 这里和之前的mal共享的同一套前端

```
server {
       listen 10001 ssl;

       ssl_certificate  /etc/nginx/cert/certificate.pem;
       ssl_certificate_key      /etc/nginx/cert/key.pem;


       location ~ ^/api/v2/ {
                include proxy_params;
                proxy_pass http://unix:/home/dell/mal_v2/mal.sock;
       }

       location ~ ^/ {
                root /var/www/html/mal_ui;
                index index.html;
       }
}
```

## supervisorctl cheathsheet

```
help # print a list of available actions
add <name> [...] # activates any updates in config for process/group
remove <name> [...] # removes process/group from active config 
update # reload config and then add and remove as necessary
clear <name> # clear a process’ log files
clear <name> <name> # clear multiple process’ log files
clear all # clear all process’ log files
fg <process> # connect to a process in foreground mode Press Ctrl+C to exit foreground
pid # get the PID of supervisord
pid <name> # get the PID of a single child process by name
pid all # get the PID of every child process, one per line
reload # restarts the remote supervisord
reread # reload the daemon’s config‐uration files, without add/remove (no restarts)
restart <name> # restart a process
restart <gname>:* # restart all processes in a group
restart <name> <name> # restart multipleprocesses
restart all # restart all processes
signal No # help on signal
start <name> # start a process
start <gname>:* # start all processes in a group 
start <name> <name> # start multiple processes
start all # start all processes
status # get all process status
status <name> # get status on a single process by name 
status <name> <name> # get status on multiple named processes
stop <name> # stop a process
stop <gname>:* # stop all processes in a group
stop <name> <name> # stop multiple processes or groups
stop all # stop all processes
tail [-f] <name> [stdout|stderr] # output the last part of process logs
```