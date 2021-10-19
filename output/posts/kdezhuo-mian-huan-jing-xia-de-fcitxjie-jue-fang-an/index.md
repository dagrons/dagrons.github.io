<!--
.. title: kde桌面环境下的fcitx解决方案
.. slug: kdezhuo-mian-huan-jing-xia-de-fcitxjie-jue-fang-an
.. date: 2021-10-08 11:29:54 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

## 目标环境

```
pve7.0 
```

## 安装fcitx

```bash
apt install fcitx fcitx-googlepinyin fcitx-table-wbpy fcitx-pinyin fcitx-sunpinyin
```

## 添加language support

System Settings > Language > add languages > 简体中文


## 添加fcitx自启动

System Settings > startup and shutdown > auto-start > add program > fcitx

## fcitx-diagnose

fcitx-diagnose会检查系统配置, 进行有效诊断, 很多时候看fcitx-diagnose就能解决问题
```bash
fcitx-diagnose
```

然后注销重新登录，在右下角fcitx设置中添加Google Pinyin即可
