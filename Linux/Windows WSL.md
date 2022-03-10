# 常用命令

```powershell
# 查看运行状态
 wsl.exe --list --verbose
```

# Windows10 WSL Ubuntu 忘记 root 密码如何重置

关闭 Ubuntu 窗口

打开 Powershell 或 cmd， 以 root 默认登陆 wsl -u root。

别关，在这个 cmd 窗口内（重点）输入 wsl 进入，

输入 passwd your_username ，确认密码。

关闭 WSL。 exit

# WSL 安装 mongodb

[install mongodb](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database#install-mongodb)

# wsl2 配置使用 windows 网络代理

wsl2 获取 win10 ip

`cat /etc/resolv.conf|grep nameserver|awk '{print $2}' => 例如：172.22.176.1`
注：由于 windows 防火墙的存在，此时可能出现 ping 172.22.176.1 失败
新建防火墙入站规则

打开控制面板\系统和安全\Windows Defender 防火墙
点击入站规则->新建规则
规则类型：自定义
程序：所有程序
协议和端口：默认即可
作用域：
本地 ip 处选择“任何 IP 地址”
远程 ip 处选择“下列 IP 地址”，并将 wsl2 的 IP 添加进去。（请根据自己 wsl2 的 ip 进行计算，我这里添加了 172.22.176.0/20）（掩码一般是 20 位）
操作：允许连接
配置文件：三个全选
名称描述：请自定义
注意：这一步完成后，从 wsl2 ping 主机的 ip 应该可以 ping 通了。
防火墙配置

打开控制面板\系统和安全\Windows Defender 防火墙\允许的应用。
将与代理相关的应用程序均设置为：允许其进行专用、公用网络通信。
特别注意的是：将 Privoxy 也配置为允许
windows 端代理软件配置

启用“允许来自局域网的连接”
测试

在 wsl2 中配置 http 代理，如 `export http_proxy="http://172.22.176.1:7079"`。注意：端口号请结合自己的代理设置进行修改
执行命令 curl cip.cc 查看 ip 地址
后面可以采用一些脚本方便代理的设置和取消
————————————————
版权声明：本文为 CSDN 博主「yann_qu」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/nick_young_qu/article/details/113709768

# Linux 安装 nodejs

https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04

# 释放 win10 子系统 WSL2 的磁盘空间

https://loesspie.com/2021/01/27/wsl2-compact-disk-win10/

```powershell
wsl --shutdown

diskpart  # 进入


select vdisk file="your ext4.vhdx  path"

compact vdisk

detach vdisk

```

# wsl 备份与恢复

```powershell
wsl -l
适用于 Linux 的 Windows 子系统分发版:
Ubuntu-18.04 (默认)
。。。

# 停止wsl
wsl --shutdown

# 备份指定系统到指定位置
wsl --export Ubuntu-18.04 d:\Ubuntu-18.04.tar

# 还原指定系统
wsl --import Ubuntu-18.04 d:\Ubuntu-18.04.tar

```
