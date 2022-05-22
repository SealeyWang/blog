# disconnect 场景一

现象： 客户端输入时 偶尔断开与服务器的链接。 客户端 pad 版本，无法上传到服务器

原因：
客户端定时刷新 session 时，更新的 session 中的文档版本

客户端的 - 给服务器 版本发送 1000 后，没有收到确认前，刚好发送了刷新 session 的消息，更新了服务端 pad.session 的版本(1001) 。
此时服务端认为客户端 pad 版本为 1001 然后给他发了 1002 版本。客户端收到 1002 版本后。判断不是当前版本 1000 + 1 ，断开链接。后续所有版本不上传。一直在 committing，20 秒后提示 slcommit

# disconnect 场景二

客户端发送大量数据到服务端时，随着数据量越来越大，收到的数据的 ACCEPT_COMMIT 越来越慢（但好像服务器处理的不慢），如图 82 的变更 37 秒发送， 服务器已经返回了 ACCEPT_COMMIT

而客户端 websocket 47 秒时才收到 ACCEPT_COMMIT

后端从[2022-04-18 11:43:38.881]接收到请求，[2022-04-18 11:43:39.009]处理结束，不到 1S

[2022-04-18 11:43:39.001] [INFO] access - [耗时统计][requestuuid:ecbdee3d-da5a-4f8a-9dde-ba940c0bc5b1][padID: undefined 发送 changeSet 结果][单个发送开始] return_msg:{"type":"COLLABROOM","data":{"type":"ACCEPT_COMMIT","newRev":82}} time:1650253419001

39S 的时候已经返回了
