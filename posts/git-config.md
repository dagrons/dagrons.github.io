<!--
.. title: Git Config
.. slug: git-config
.. date: 2023-09-14 00:10:54 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

gitconfig 

```conf
[user]
email = heyuehuii@126.com
name = dagrons
[alias]
lg = log --decorate --oneline --graph
lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)'
lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)'
[core]
	autocrlf = input
```