1-1 nginx 高效 可靠 代理中间件。
1. nignx 配置场景
代理服务
动态缓存
动静分离
负载均衡
nginx&lua开发
...
1. 了解中间件架构
sql注入防攻击
对请求访问的控制
对请求频率控制
对防爬虫
1. nginx性能优化
http性能压测
性能瓶颈分析
系统性能优化
基于nginx的性能配置优化

1-2 准备
1. CPU >= 2Core 内存 >=256M
1. 操作系统 版本>=7.0 位数x64
1. 环境确认：
  1. 系统网络 ping www.baidu.com
  1. yum 可用 [root@iZwz9g1c3fleikf5q63pztZ ~]# yum list|grep gcc
  1. 关闭iptables规则 查看iptables -L/ iptables -t nat -L 关闭 iptables -F
  1. 停用selinux 查看getenforce 关闭 setenforce 0
1. 安装确认：
基本库：
yum -y install gcc gcc-c++ autoconf pcre pcre-devel make automake
基本工具
yum -y install wget httpd-tools vim tree

1. 初始化目录
cd /opt mkdir app(code) download logs work(shell脚本) backup

基础篇
2-1 什么是nginx
开源、高性能、可靠 HTTP 中间件、代理服务

2-2 常见中间件服务
HTTPD Apache基金会
IIS 微软
GWS Google 

2-3 nginx 优势多路IO复用
W#Techs.com

难点-阻塞：解决方案
1. 多进程多线程处理
1. 一个线程 多路IO 复用 （高效）

2-4 ngnix epoll 模型

实现IO 流非阻塞模式
while true {
  for i in stream[]; {
    if i has data
    read until unavailbale
  }
}
如果所有的流都没有数据，那么只会浪费CPU

IO复用内核模式：
类型一 ：select类型、poll模型
类型二：epoll 模型

select -> poll -> epoll

select 模型
while true {
  select(stream[]) -> 查找出存在数据的数据流
  for in in stream[]{
    if i has data
    return until unavailable
  }
}

epoll 模型
1. 解决selec模型对于文件具柄FD打开限制 （2048限制）
1. 采用callback函数回调机制优化模型效率

2-5 nginx cpu 亲和