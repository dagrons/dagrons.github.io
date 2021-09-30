<!--
.. title: proxmox-宝宝的第一个homelab解决方案
.. slug: proxmox-bao-bao-de-di-yi-ge-homelabjie-jue-fang-an
.. date: 2021-09-30 17:11:58 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

## 前言

- 由于之前接触linux比较多, 然后又听yxc大佬说云计算是趋势, 然后机缘巧合接触了lxc这种, 在所有东西水到渠成之后, 好像发现自己已经走向了homelab玩家的道路, 索性就上promox(pve), 一个完备的homelab解决方案

- 所以习惯性的算homelab入门吧, "hello, world!"

## 基础概念扫盲

- datacenter: 顶级概念, 其实没啥意义
- node: 在pve中指的是一个物理机
- cluster: 在pve中指的是物理机的集合, 组合提供计算资源
- pod: 在k8s中指的是一个基础服务单元

## 安装

- 不对安装过程做介绍了, 太小白了, proxmox iso + balenaEtcher搞定


## 网络设置

由于初始填写的ip是非法的, 然后又不能选dhcp, 所以需要先对网络进行一定设置, 选择dhcp

```
# /etc/network/interfaces

auto lo
iface lo inet loopback

iface enp2s0 inet manual

auto vmbr0
iface vmbr0 inet dhcp
        bridge-ports enp2s0
        bridge-stp off
        bridge-fd 0
```

然后

```
systemctl restart networking
```

## 概览

好多东西可能需要以后慢慢填, 所以在这里介绍一下proxmox到底是个什么吧


不多说, 先上灵魂画图

<img src="/images/cloud-computing.png" style="text-align: center" />


总的来说, 这个东西其实提供了一个搭建集群的框架


然后, 放张UI图吧

<img src="/images/cloud-storage.png" style="width: 50%; height: 50%; text-align: center" />

几个重要的点:

- Cluster: 这里用来管理集群
- Ceph: 分布式的文件系统
- Storage: 管理所有的文件系统, 包括非分布式文件系统

## 其他

没有其他了