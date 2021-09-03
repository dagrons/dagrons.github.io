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

# 先创建上传ip的脚本
/usr/local/bin/upload-ip内容如下:
```bash
#!/bin/bash

while true; do
    ipaddr=$(ifconfig br0 |egrep 'inet\ '|awk '{print $2}')
    host="实验室服务器3"
    time=$(date)
    cat <<EOF | ssh ubuntu@tencent_cloud 'cat > ~/lab_host3'
host: $host
ip: $ipaddr
update time: $time
EOF
    sleep 60
done
```

# 创建service文件
/etc/systemd/system/upload-ip.service内容如下：
```
[Unit]
Description=Send ip to remote tencent cloud server
After=network-online.target
Wants=network-online.target

[Service]
User=dell
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
```

# 附送systemctl cheatsheet
```
systemctl daemon-reload # Reload the service files to include the new service
systemctl start your-service.service # Start your service
systemctl status your-service.service # To check the status of your service
systemctl enable your-service.service # To enable your service on every reboot
systemctl disable your-service.service # To disable your service on every reboot
```

