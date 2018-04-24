title: 插桩分析
---

# 插桩分析
使用流程
* 将待测试的 apk 上传到服务端进行插桩
* 下载插桩后的 apk
* 安装插桩后的应用，授权，进行测试流程（自动化测试，人工测试都可以），log会存在手机本地
* 将设备通过USB连接到开发机，并使用本客户端将 log 上传至服务端进行分析
* 通过[Appetizer Desktop](https://appetizer.io) >= 1.2.0进行可视化查看报告

插桩和分析包括
* 应用崩溃（Crash）的原因和崩溃时的状态
* 所有线程抛出的异常（即使不会造成崩溃）
* 应用未响应（ANR）的状态
* HTTP 请求以及回复的详细信息，包含以下库的API (更多库支持正在添加)
    - [okhttp >=2.2 或 okhttp3](https://github.com/square/okhttp/wiki/Interceptors#availability)
    - [retrofit >1.8（基于okhttp的）](https://github.com/square/okhttp/wiki/Interceptors#availability)
    - apache http
    - urlconnection
    - volley
* 主线程卡顿（图片问题，回调问题等）
* 界面切换耗时
* CPU 占用率和 heap 占用大小
