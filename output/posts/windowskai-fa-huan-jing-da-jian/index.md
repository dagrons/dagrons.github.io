<!--
.. title: Windows开发环境搭建
.. slug: windowskai-fa-huan-jing-da-jian
.. date: 2023-08-13 21:53:33 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->
## WSL 
只能在windows上被迫营业了，先[安装个wsl2](https://zhuanlan.zhihu.com/p/266585727)缓一缓，在升级前记得开启hyper-v 

wsl2本质上是一个和windows高度集成的虚拟机（所以需要hyper-v）

然后安装[systemd](https://github.com/badgumby/arch-wsl)

host的网络问题可以配置代理到macos，保证macos没有熄屏才能上网

## 配置nvm

得先解决网络问题，nvm和npm都可以配置国内源

## 配置jdk

```bash
pacman -S jdk-openjdk8
```

## 配置vsode
插件：
  - Awesome Emacs Keymap
  - Emacs Keymap
  - Find-Jump 
  - WSL 
  - vue
  - Babel JavaScript 
  - webpack 
  - ESlint 
  - Debugger for Firefox
  - Microsoft Edge Tools for VS Code 

## offline docs 

devdocs支持offline data，神器啊


