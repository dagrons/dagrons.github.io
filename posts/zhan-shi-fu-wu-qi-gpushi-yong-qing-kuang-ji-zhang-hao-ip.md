<!--
.. title: 展示服务器GPU使用情况及账号IP
.. slug: zhan-shi-fu-wu-qi-gpushi-yong-qing-kuang-ji-zhang-hao-ip
.. date: 2021-09-03 20:45:38 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

# 问题分析：

1. 首先通过crontab设置定时任务，将ip信息和gpu使用情况定时打到某文件中
2. 通过one-shot-server创建服务


# GPU使用情况脚本
```bash
#!/bin/bash

persons=()
usages=()

for t in $(nvidia-smi|egrep '^\|[\ ]{4}[[:digit:]]' | tr -d '|'|awk '{print $4}'); do # command substitution
    persons+=("$(cat /proc/$t/cgroup|head -n 1|cut -d '.' -f 3)")
done

for t in $(nvidia-smi|egrep '^\|[\ ]{4}[[:digit:]]' | tr -d '|'|awk '{print $7}'); do 

    usages+=("$t")
done

plen=${#persons[@]} # parameter expansion

echo "------"
echo "usage:"
echo "------"
echo 
for ((i=0; i<${plen}; i++)); do # arithemetic expansion
    printf "%-10s\t | %10s\n" ${persons[i]} ${usages[i]}
done
echo 
echo "update at $(date)"
echo "------------------------------"
echo "------------------------------"
echo
echo 
```

# 账号IP信息打印
```bash
lxc list 
```

# crontab设置定时任务

定时打印ip和gpu使用信息

```
*/1 * * * * nvidia-smi > /dagongren/share/status
*/1 * * * * /root/who_is_using_gpu.sh > /dagongren/share/usage
*/1 * * * * /root/who_is_using_gpu.sh > /dagongren/share/ip && /snap/bin/lxc list >> /dagongren/share/ip
```

# one-shot-server脚本
```bash
#/bin/bash

PORT=$1
FILE=$2

if [[ -z "${FILE}" ]]
then
  echo "The file ${FILE} does not exist"
  exit 1
fi

echo "The file ${FILE} does exist"
while [[ 1 ]]
do
  { echo -ne "HTTP/1.0 200 OK\r\nContent-Length: $(wc -c <${FILE})\r\n\r\n"; cat ${FILE}; } | nc -l -p ${PORT}
done
```


# 通过one-shot-server启动服务
> 最好启动两次, 我也不知道为什么
```bash
nohup bash ./one_shot_server 8080 /root/share/ip > /dev/null & 
```

