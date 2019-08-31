# RaspberryPi

## 1. 刻录系统

准备工作

* 树莓派
* SD卡：树莓派的SD卡口在底部
* [树莓派镜像](https://www.raspberrypi.org/downloads/raspbian/%20)
* [BalenaEtcher](https://www.balena.io/etcher/)：刻录工具
* 读卡器：电脑读SD卡

利用`BalenaEtcher` 将`树莓派镜像`傻瓜式的刻录到`SD卡`即可。

## 2. 初始化

刻录完毕后得到了一个基本的系统，可以接上各类外设\(显示器、键盘、硬盘\)就可以当一台电脑了

这里我们打算将树莓派作为一个云主机\(内网的~\)，最终其只接一个电源即可

### 开放ssh登录

利用命令行控制树莓派才是常态！

在刻录的SD卡的根目录（刻录完会被挂载为boot）新建一个名为ssh的文件即可！

> touch ssh

默认账号

* 用户名：pi
* 密码：raspberry

### 无线连接

在根目录建立`wpa_supplicant.conf`

由于bit坑爹的网络，这里创建了三个网络

1. BIT-Mobile：利用用户名和账号同时进行认证的网络
   * ssid：网络名称
   * identity：账号
   * password：密码
   * priority：网络优先级
2. 无密码网络
   * key\_mgmt：加密方式
3. 有密码网络
   * psk：密码

```text
country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="BIT-Mobile"
    key_mgmt=WPA-EAP
    pairwise=CCMP TKIP
    group=CCMP TKIP
    eap=PEAP
    identity="xxx"
    password="yyy"
    phase1="peaplabel=auto pepver=auto"
    phase2="MSCHAPV2"
    priority=5
}

network={
    ssid="BIT-Web"
    key_mgmt=NONE
    priority=4
}

network={
    ssid="sola"
    psk="zzz"
    priority=3
}
```

在网络连接成功后，可以通过外接的显示器\(HDMI口\)，从开机画面上看到具体的IP

至此，我们得到了一个能连上Internet的微型计算机了！

## 3. 骚操作

### raspi-config

> sudo raspi-config

开启各种针对树莓派的定制的操作

* VNC：远程可视化界面
* SSH

### 更改源

修改 `/etc/apt/sources.list`

> deb [http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/](http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/) stretch main non-free contrib
>
> deb-src [http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/](http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/) stretch main non-free contrib

读取新的源，更新缓存的软件列表

> sudo apt-get update

升级软件\(一般不用\)

> sudo apt-get upgrade

