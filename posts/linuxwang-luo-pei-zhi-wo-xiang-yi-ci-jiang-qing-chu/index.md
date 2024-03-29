<!--
.. title: linux网络配置-我想一次讲清楚
.. slug: linuxwang-luo-pei-zhi-wo-xiang-yi-ci-jiang-qing-chu
.. date: 2021-09-30 11:13:07 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

## 前言

- linux网络配置复杂, 配置文件繁多, 因此经常产生困扰, 所以我想在这里一次性梳理好

## 简介

Linux的网络工具架构如下:

<img src="/images/networking.png" style="width: 50%; height: 50%; text-align: center" />

总的来说, 主要有两套backend, 一个是network-manager, 一个是system-networkd, 

**network-manager vs system-networkd**
network-manager主要用于GUI环境下的网络配置, system-networkd主要用于server环境下的网路配置, 但作用是一样的, 在netplan的术语中, 它们只是render, 即读取配置文件, 将需要配置的网络环境渲染出来


## 配置文件

**传统配置文件**

/etc/network/interfaces是system-networkd的配置文件, 下面是一个典型的interfaces配置

```
# lab_host2:/etc/network/interfaces
auto lo
iface lo inet loopback

auto br0
iface br0 inet dhcp
    bridge-ports enp0s31f6    

iface enp0s31f6 inet manual
```


nmcli: 是network-manager的命令行配置工具

**netplan**

netplan的好处在于为system-networkd和network-manager提供了一致的配置文件, 从而避免两个render间的冲突, 在netplan中, 我们可以通过renderer关键词指定renderer, 除此之外的语法都是一致的

如下, 是一个典型的netplan配置文件, 指定renderer为networkd

```yaml
# lab_host3:/etc/netplan/01-netcfg.yaml
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
