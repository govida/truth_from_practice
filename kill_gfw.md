# Kill GFW

突破长城！

## 1. 搬瓦工

首先你得有潜入国外的帮手~

baidu一下你就知道，不行就bing一下~

### SSH端口被封

1. 利用web界面登陆shell
2. 修改端口（在最后一行`shift`+`g`）

   > vim /etc/ssh/sshd\_config

3. 重启ssh（两种方案）
   * reboot
   * /etc/init.d/ssh restart

## 2. shadowsocks

### 安装

在境外服务器上的搭建服务端：

1. 下载傻瓜式脚本

   > wget --no-check-certificate -O shadowsocks-all.sh [https://raw.githubusercontent.com/teddysun/shadowsocks\_install/master/shadowsocks-all.sh](https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh)

2. 执行傻瓜脚本，一路default选项下来也可以

   > sh shadowsocks-all.sh

最后记录：

* IP
* 端口
* 密码

### 使用

在自己的机器上就可以利用上述账号登陆shadowsocks，shadowsocksX-NG——[传送门](https://github.com/shadowsocks/ShadowsocksX-NG)

## 3. kcptun

一般来说，搬瓦工的每月提供的流量用不完，而且仅用shadowsocks的翻墙网速有限，kcptun应运而生，可以利用额外的流量换取更高的浏览速度

详细原理——[传送门](https://github.com/xtaci/kcptun)，没读过~

### 安装

在境外服务器上的搭建服务端：

1. 下载傻瓜式脚本

   > wget --no-check-certificate -O kcptun.sh [https://github.com/kuoruan/shell-scripts/raw/master/kcptun/kcptun.sh](https://github.com/kuoruan/shell-scripts/raw/master/kcptun/kcptun.sh)

2. 执行傻瓜脚本，认字就好，注意以下几点：
   * 正确填写shadowsocks的端口（详见上节）
   * 记录kcptun的服务端口
   * 记录最后给出的以`key=`开头的命令参数

### 使用

1. 替换端口，将原来的shadowsocks的端口，替换为kcptun端口
2. 启用插件
   * 插件：填写`kcptun`
   * 插件选线：填写以`key=`开头的命令参数

参考链接

* [使用搬瓦工搭建 ShadowSocks 翻墙（VPN）](https://depthlove.github.io/2019/03/29/establish-vpn-server/)

