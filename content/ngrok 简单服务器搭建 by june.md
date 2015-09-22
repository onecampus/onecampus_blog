+++
date = "2015-09-20T10:14:34+08:00"
draft = false
title = "ngrok 简单服务器搭建 by june"

+++

## ngrok介绍
```
  ngrok 是一个反向代理，通过在公共的端点和本地运行的 Web 服务器之间建立一个安全的通道。ngrok 可捕获和分析所有通道上的流量，便于后期分析和重放(来自百度百科)。通过ngrok,我们可以在外网轻松访问本机的资源(其中一个非常重要的功能)。在微信公众号等的开发中，ngrok将大大节省开发者的调试，测试时间和成本,可谓神器!
  
```
## 	Mac 搭建ngrok服务器
```
1.下载ngrok,地址：https://ngrok.com/download。 请注意:下载官方1.x客户端，官方目前并不准备开放2.0客户端的第三方服务器支持。安装了brew的用户可直接 brew install ngrok,中途会提示安装go,选择是就行了，简单粗暴.

2.去官网注册一个账号并获取密钥，地址：https://ngrok.com/

3.运行$ ngrok -authtoken YOUR_TOKEN_HERE 80（这是一个默认的配置，默认访问：http://127.0.0.1）ngrok -authtoken只需要运行一次，以后都不需要再次运行, 端口可以随便改

4.配置一级域名：$ ngrok -subdomain = june(任何你喜欢的英文) 80

5.由于ngrok默认是部署在国外的服务器，很多时候都不能访问或者访问很慢，因此，我们可以使用部署在国内的服务器， 详情请访问:http://www.tunnel.mobi/
```

## 更多信息
访问 http://www.tunnel.mobi/, https://ngrok.com/docs
