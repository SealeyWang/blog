# Windows10 WSL Ubuntu 忘记 root 密码如何重置

关闭 Ubuntu 窗口

打开 Powershell 或 cmd， 以 root 默认登陆 wsl -u root。

别关，在这个 cmd 窗口内（重点）输入 wsl 进入，

输入 passwd your_username ，确认密码。

关闭 WSL。 exit
