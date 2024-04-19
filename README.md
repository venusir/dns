
### 若在PVE LXC容器中部署，需开启容器的TUN 

在 pve 宿主中, 确认 `/dev/net/tun` 存在并获取对应的信息, 具体命令和返回如下

```
root@pve:~# ls -al /dev/net/tun
crw-rw-rw- 1 root root 10, 200 Jun 30 23:08 /dev/net/tun
```

记录其中的 `10, 200` 这两个数字, 后面需要用到.

然后修改 `/etc/pve/lxc/CTID.conf` 文件, 新增如下两行

```
lxc.cgroup2.devices.allow: c 10:200 rwm
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```

上面的 `10:200` 需要和前面使用 `ls -al /dev/net/tun` 获取的结果对应起来.

### 开启IP转发

开启 lxc 的 IP 转发功能

编辑 `/etc/sysctl.conf` 文件, 将以下两行的注释去掉. 如果没有这两行, 需要添加

```
net.ipv4.ip_forward=1
net.ipv6.conf.all.forwarding=1
```

编辑完成后, 使用 `sysctl` 命令 reload

```
sysctl -p /etc/sysctl.conf
```

### 视频地址

[利用debian搭建软路由&adhome拦截广告&mosdns分流&clash或者v2ray代理](https://www.youtube.com/watch?v=jGR1LE7Bdf0)

[ROS搭配Clash，彻底抛弃openwrt，配合无污染DNS进行IP分流](https://www.youtube.com/watch?v=eOr8yrp4KWk&t=777s)

[RBC第二期，ROS搭配Clash meta建立透明网关，进行IP分流](https://www.youtube.com/watch?v=Tnl75GunY9w)

### 一键安装

```
bash <(curl -sSL https://raw.githubusercontent.com/venusir/dns/main/mihomo/mihomo.sh)
```

```
bash <(curl -sSL https://raw.githubusercontent.com/venusir/dns/main/mosdns/dns.sh)
```