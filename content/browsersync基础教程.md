+++
Categories = ["Development", "GoLang"]
Description = ""
Tags = ["Development", "golang"]
date = "2015-10-30T10:21:57+08:00"
menu = "main"
title = "browsersync基础教程"

+++
##browser-sync基础配置
> [browser-sync](http://www.browsersync.io/)是个啥？  

browser-sync是一个前端辅助性插件，用于开发时自动刷新页面。

> 这玩意有什么优势？

1. 不需要写任何代码，直接几个命令就能控制监听自动刷新，很爽有木有？
2. 响应式开发时可能需要在多种设备或者屏幕上测试，browser-sync能一次性帮你刷新所有页面，简直是懒人必备啊（啥？什么是响应式？拜拜）。

> how to install?

1. 需要npm环境，这个下载安装[nodejs](https://nodejs.org/en/)之后就有了
2. npm install -g browser-sync

> 问： 我已经有个服务器eg: localhost:4200，我想用browser-sync来监听文件的变化该怎么做？

1. 开启你的服务器并确保ip不是3000(3000被占用的话browser-sync会开启失败)
2. browser-sync start --proxy "localhost:4200" --files "js/.js,css/.css"  
   --proxy: 启动一个内置的服务器  
   --files: 要监听变化的对象（如果实在不知道要监听什么，可以这样写**,不过不推荐这么做..）

> 个人感觉browser-sync美中不足的地方

1. 监听对象是css文件自动注入不刷新页面的机制才会生效，但监听对象是诸如scss一类文件的时候注入就不生效了，页面还是会刷新。

