<!--
.. title: 自动上传实验室服务器信息到腾讯云服务器
.. slug: zi-dong-shang-chuan-shi-yan-shi-fu-wu-qi-xin-xi-dao-teng-xun-yun-fu-wu-qi
.. date: 2021-09-03 19:31:01 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

因为实验室服务器每次重启IP都可能改变，因此需要服务器重启后自动将IP上传到腾讯云服务器上，这样就不用每次连显示器去查询IP（十分麻烦）

# 问题分析

1. 服务器无法自动连接到Internet，就无法连接到腾讯云，所以需要一个服务监听Internet连接状态，在没有Internet连接时，自动登陆校园网
2. 为了方便，我们选择用ssh协议上传ip信息，并且60秒更新一次

记录流程如下：

# 先将腾讯云IP加到/etc/hosts
```bash
echo "120.53.125.30     tencent_cloud" >> /etc/hosts
```

# 将ssh-key pubkey加到tencent_cloud的信任列表中
以一步是防止ssh上传ip时要求输入密码
```bash
cat ~/.ssh/id_rsa.pub | ssh ubuntu@tencent_cloud 'cat >> ~/.ssh/authorized_keys' 
```

# 检测Internet连接并自动登陆校园网的脚本
/usr/local/bin/autologin内容如下
```bash
#!/bin/bash

while true; do
    if ping -q -c 1 -W 1 8.8.8.8 >/dev/null; then
	echo "Internet Connection is alive"
	sleep 10
    else
	python3 /home/dell/get2internet.py 2020110966 Daxiahyh1
    fi
done
```

# 上传ip的脚本
/usr/local/bin/upload-ip内容如下:
```bash
#!/bin/bash

while true; do
    ipaddr=$(ifconfig br0 |egrep 'inet\ '|awk '{print $2}')
    time=$(date)
    cat <<EOF | ssh root@tencent_cloud 'cat > /var/www/html/lab_host3.html'
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
</head>
<body>
<h1>基础信息</h1>
<p>
服务器编号： 3
</p>
<p>
服务器IP：$ipaddr
</p>
<p>
账号IP及GPU使用情况: <a href="http://$ipaddr:1205">入口</a>
</p>
<p>update time: $time</p>
<h1>README</h1>
<h2>硬件信息</h2>
<p>
GPU: 两块3080(公用) <br />
CPU: Intel(R) Xeon(R) Silver 4210R CPU @ 2.40GHz <br />
</p>
<h2>使用注意</h2>
<p>
<ul>
<li>
服务器IP在重启后IP可能变化, 但这个链接不会变化, 要是发现服务器登不上, 可以点击上面的账号IP信息入口, 查看自己账号IP是否改变过
</li>
<li>
/share下是公用目录, 建议将数据集放在这(这样别人也可以用), 以及和别人共享文件, 也是这个目录, 显卡驱动安装脚本(rescue.sh)也在这里, 最好不要自己瞎装驱动了, 如果要自己装, 后面加上--no-kernel-module参数
</li>
<li>
账号默认root密码是1205, ssh端口是42502, 最好改一下密码
</li>
<li>
如果要对自己的容器进行一些奇奇怪怪的操作之前, 可以提前让我给你容器创建个备份, 不然可能发生一些不太好的事情
</li>
</ul>
</p>
<body>
</html>
EOF
    sleep 60
done

```

# 创建autologin.service文件
/etc/systemd/system/autologin.service内容如下：
```
[Unit]
Description=Auto Login for Internet
After=network-online.target
Wants=network-online.target

[Service]
User=root
WorkingDirectory=/
ExecStart=/usr/local/bin/autologin
Restart=always

[Install]
WantedBy=multi-user.target
```

# 创建upload-ip.service文件
/etc/systemd/system/upload-ip.service内容如下：
```
[Unit]
Description=Send ip to remote tencent cloud server
After=network-online.target
Wants=network-online.target

[Service]
User=root
WorkingDirectory=/
ExecStart=/usr/local/bin/upload-ip
Restart=always

[Install]
WantedBy=multi-user.target
```

# 启动服务
```bash
sudo systemctl deamon-reload
sudo systemctl start upload-ip.service
sudo systemctl enable upload-ip.service
sudo systemctl start autologin.service
sudo systemctl enable autologin.service
```

# 附送systemctl cheatsheet
```
systemctl daemon-reload # Reload the service files to include the new service
systemctl start your-service.service # Start your service
systemctl status your-service.service # To check the status of your service
systemctl enable your-service.service # To enable your service on every reboot
systemctl disable your-service.service # To disable your service on every reboot
```

