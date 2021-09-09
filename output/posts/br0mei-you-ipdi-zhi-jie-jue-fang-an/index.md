<!--
.. title: br0没有ip地址解决方案
.. slug: br0mei-you-ipdi-zhi-jie-jue-fang-an
.. date: 2021-09-09 14:14:32 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

# 问题分析
1. br0没有ip，但enp0s31f6有ip
2. lxc list显示容器都没有ip, 推测br0没有正常工作
3. dhclient br0 无法成功
4. brctl show br0 在interfaces列没有enp0s31f6

# 解决方案

首先通过brctl show br0发现没有物理网卡绑定，因此通过
```bash
brctl addif br0 enp0s31f6
```
手动添加物理网卡, 添加后发现容器可以正常获取ip了

但host的br0仍然没有ip

然后重新修改/etc/netplan/01-netcfg.yaml

```
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s31f6:
      dhcp4: yes
      dhcp6: yes
  bridges:
    br0:
      dhcp4: yes
      dhcp6: yes
      interfaces:
        - enp0s31f6
```

重新载入配置
```bash
netplan --debug apply
```

发现br0成功得到ip

重启后依然生效