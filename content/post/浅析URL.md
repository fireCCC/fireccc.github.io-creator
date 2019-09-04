---
title: "浅析URL"
date: 2019-09-03T13:34:36+08:00
draft: false
---

## 一 URL 包含哪几部分，每部分分别有什么作用

#### url全称 Uniform Resource Locator 统一资源定位符
1. 协议 http/https
2. 域名 www.so.com
3. 服务器路径 /s
4. 查询参数 wd=hello&rsv_spt=1
5. 锚点 #5 与浏览器页面有关

## 二 DNS 的作用是什么，nslookup命令怎么用

#### DNS全称Domain Name System 域名系统
1. 当输入xiedaimala.com, 浏览器回向电信/联通提供的DNS服务器询问xiedaimala.com的对应IP
2. 电信/联通会回答一个IP
3. 浏览器向对应IP的80/443端口发送请求
4. 请求内容是查看xiedaimala.com的首页

#### nslookup命令用于查询DNS的记录，查看域名解析是否正常，在网络故障的时候用来诊断网络问题。 

`nslookup baidu.com`
```
DNS request timed out.
    timeout was 2 seconds.
服务器:  UnKnown
Address:  4.2.2.1

非权威应答:
名称:    baidu.com
Addresses:  220.181.38.148
          39.156.69.79
```
`nslookup domain [dns-server]`
- 查询一个域名的A记录

## 三 IP的作用是什么，ping命令怎么用

只要在互联网中，就会有至少一个ip<br>
ip分为内网ip和外网ip<br>
路由器连上电信的服务器就会被分配一个外网ip,如果重启路由器可能被重新分配一个外网ip<br>
路由器会给自己分配一个好记的内网IP,比如192.168.1.1<br>
路由器会在一定范围内创建一个内网,给内网中的每一个设备分配一个不同的内网IP<br>
一般来说格式是192.168.xxx.xxx,比如192.168.1.2<br>

#### ping命令
ping 命令有助于验证网络层的连通性！一般进行网络故障排除时，可以使用ping 命令向目标计算机或IP地址发送ICMP回显请求，目标计算机会返回回显应答，如果目标计算机不能返回回显应答，说明在源计算机和目标计算机之间的网路存在问题，需要进一步检查解决！<br>
ping 命令是Windows 操作系统中集成的一个TCP/IP协议探测工具，它只能在有TCP/IP协议有网络中使用。<br>
ping 命令的格式为：ping[参数1][参数2][……][目的地址]<br>
如果不知道ping命令有那些参数的话，只要在命令提示符中键入ping命令，就能得到。<br>
一般使用只需要在命令行输入ping baidu.com即可等待输出连通性测试。<br>

正在 Ping baidu.com [220.181.38.148] 具有 32 字节的数据:<br>
来自 220.181.38.148 的回复: 字节=32 时间=78ms TTL=46<br>
来自 220.181.38.148 的回复: 字节=32 时间=63ms TTL=46<br>
来自 220.181.38.148 的回复: 字节=32 时间=61ms TTL=46<br>
来自 220.181.38.148 的回复: 字节=32 时间=62ms TTL=46<br>

220.181.38.148 的 Ping 统计信息:<br>
    数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，<br>
往返行程的估计时间(以毫秒为单位):<br>
    最短 = 61ms，最长 = 78ms，平均 = 66ms<br>

## 四 域名是什么，分别哪几类域名

#### 域名就是对IP的别称

一个域名可以对应不同IP,称为负载均衡,防止一台机器扛不住<br>
一个IP可以对应不同域名,称为共享主机,一般没钱就会这样做<br>

#### 域名分为顶级域名 二级域名 三级域名

com 顶级域名<br>
xiedaimala.com 二级域名<br>
www.xiedaimala.com 三级域名<br>
他们是父子关系<br>
www是多余的吗?是的，非常多余:)



