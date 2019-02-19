title: 快速入门
---
# 快速入门（5分钟）

> 什么样的工程师没药救？ 只会用百度 翻墙也不舍得翻 页内ctrl+f 也不会 就会干用眼睛看 搜索seo 关键词 也不会 好的站内搜索工具也都不用 就是干用百度查 — testerhome [小马 @harsayer](https://testerhome.com/harsayer)

## 安装
* 安装JDK, Android Studio, Android SDK 可以参考 [教程](https://blog.csdn.net/hebbely/article/details/78970918)
* 到 Appetizer 主页 https://appetizer.io/cn/  注册 AppetizerIO 账号，并下载最新版本的客户端（Windows, Linux, MacOS 解压）运行Appetizer/Appetizer.exe/Appetizer.app，登录
* 首次运行注意防火墙允许，MacOS首次运行需要右键->运行
* [常见安装问题](#安装问题)


## 接入数据收集
* 插桩能为打包好的APK自动增加数据收集功能；在界面的左边主导航选择 `测试包` ，点击 `上传APK插桩`
![](get-started-instrumentation.png)
* 选择APK文件，等待插桩完成（1分钟-几分钟），在界面右上角点击刷新按钮，列表中最上方显示的为最近的插桩任务。在 `下载` 下拉菜单中选择 `直接下载` 即可下载插桩后的APK（插桩包）到电脑；也可以选择 `手机下载` 用手机浏览器扫码下载到手机
![](get-started-download-instrumented-apk.png)


## 手工测试 获取数据并上传分析
* 在待测手机上卸载原来的APP，安装插桩后的APK
* 启动后即开始测试并自动收集数据，手机屏幕右边会出现Appetizer浮动功能菜单
![](./usage/floating-menu.png)
* 对APP进行一些操作，后点击浮动功能菜单中的 `上传分析`，会有Toast提示上传成功
* 除了最基本的手工测试外，Appetizer 还支持[脚本测试和自动化工具测试](usage/testing.html)

## 查看报告
* 回到Appetizer桌面版，在主导航选择 `测试数据分析`，最新的报告在列表最上方；点击 `打开` 按钮查看；
![](get-started-select-report.png)
* 报告中会记录测试过程中遇到的错误（异常和ANR等闪退问题），性能问题，以及所有的网络请求
* 如果本报告数据较少，可以点击列表界面右上角的 `样例` 查看一次比较完整的手工测试后产生的报告

<video poster="get-started-sample-report.png" controls>
    <source src="/videos/report.mp4" type="video/mp4" />
</video>

## 安装问题
* Appetizer桌面版无法启动或者启动白屏
  * 请完全解压，不要直接在压缩包里双击执行；请注意Appetizer的组件（例如starfish.exe）不要被流氓安全软件删除或者隔离到小黑屋
* 如果在Linux上提示错误 `error while loading shared libraries: libconf-2.so.4`
  * 需要 `sudo apt-get install libgconf-2-4`
* 如果登录后弹出需要ADB，请检查Android SDK 的platform-tools是否正确安装并且是否添加到了PATH，默认adb位置在：
```
# MacOS
~/Library/Android/sdk/platform-tools
# Windows
C:\Users\Administrator\AppData\Local\Android\sdk\platform-tools
# Linux
~/Android/Sdk/platform-tools
```
* 插桩的要求
  * 被插桩的APK可以混淆，不可加固，插桩任务的状态会显示插桩中的问题，如果是服务器相关问题或者长时间显示为等待上传，请点击黄色上报按钮
* 安装插桩包的时候出错
  * 请在手机上卸载原来版本
* 插桩包启动后闪退
  * 请[重新签名插桩后的APK重试](usage/instrumentation.html#常见插桩问题排查)
  
## 继续阅读
* [选择合理的测试方法 提高测试覆盖面 让Appetizer找到更多问题](usage/testing.html)
* [理解Appetizer报告找到的问题 导出报告进一步分析](usage/analysis.html)
* [接入更多的第三方服务 从报告开始构建完整质量保障体系](usage/3rdparty.html)
* [加入QQ群 467889502 和Appetizer团队以及大家互动](http://shang.qq.com/wpa/qunwpa?idkey=4f1ef873aac8ca2a545fae99067ef5f4e505ac4a6ee3d1fad937a5dfe43ae274)

