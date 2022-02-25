本文介绍如何在 Windows 系统中连接 iOS 设备 并对 Web 页面进行真机调试
必须前提
iOS 设备、数据线
Node.js 环境
Chrome 浏览器
环境准备
安装 Node 环境
参考 Node 安装的教程，确保终端输入 node 时可正常使用

安装 scoope 以及相关配置
为了安装后续需要用的工具 remotedebug-ios-webkit-adapter
打开 win 下的 powershell（最好使用管理员权限运行）
依次输入：

    Set-ExecutionPolicy RemoteSigned -scope CurrentUser
    iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
    scoop bucket add extras
    scoop install iOS-webkit-debug-proxy
    npm install -g vs-libimobile

安装 remotedebug-ios-webkit-adapter
该调试方法最核心的部分，就是依赖该工具
打开 win 下的 powershell 或其他终端软件
npm install remotedebug-ios-webkit-adapter -g

iOS 设备点开 设置 > Safari 浏览器 > 高级 > Web 检查器，开启该选项。

iOS 设备连接电脑，信任该电脑

打开终端，执行该命令：
remotedebug_iOS_webkit_adapter --port=9000

iOS 端打开 safari 浏览器；PC 端打开 Chrome，进入 chrome://inspect/#devices 页面，并在 Discover network targets 选项添加 localhost:9000 配置。
此时刷新 iOS 页面，在 Chrome 中可看到 iOS 当前的页面地址，点击 inspect 即可进入调试页面。

提示：
在首次点击 inspect 若出现白屏，似乎要爬一下梯
————————————————
版权声明：本文为 CSDN 博主「ccczxacxzxcz」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/ruohua3kou/article/details/105172890
