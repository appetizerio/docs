title: 测试
---

# 测试

> 测试方法千千万，不要迷信任何一种；当工作只需要一次点击一个工具时，请准备下岗

## 手工测试 PC端控制
* 通过USB连接测试Android设备，在设备上启动开发者选项->USB调试，在主导航选择 `APP测试`，显示检测到已连接的设备；选择设备，选择已插桩的APK作为待测APP，如下图所示
![](../get-started-testing.png)
* 点击 `安装APP` 后，点击 `启动APP并开始测试`
![](../get-started-start-testing.png)
* 在设备上对APP进行操作，然后点击 `上传分析` 等待分析完成
![](../get-started-finish-testing.png)
* 完成测试用于放弃这次测试的数据；每次开始测试会清空之前的数据

* 优点：直接、简单、完美理解业务流程、完美模拟真实用户行为，适用于新业务（自动化未跟上），表单类业务（输入要求或者业务逻辑复杂），有特殊交互（例如特殊手势），有防止自动化的情况（例如校验码）
* 缺点：效率低下，不能批量多设备运行
* 建议：可配合录制重放减轻部分工作

## 手工测试 手机端控制
* 启动插桩后的APP自动有Appetizer浮动功能菜单，开始测试 放弃测试 上传分析
![](floating-menu.png)

## UI自动化脚本测试
* 社区有很多教学，搜索引擎和社区是学习的好地方；搜索关键字：Appium, uiautomator, Calabash, xpath
  * [python appium UI 自动化测试框架讨论](https://testerhome.com/topics/11866)
  * [浅谈自动化测试工具 python-uiautomator2](https://testerhome.com/topics/11357)
  * [UI 自动化框架调研总结](https://testerhome.com/topics/6602)
  * [网易 UI 自动化工具 Airtest 浅用记录](https://testerhome.com/topics/12391)
* 优点：高效，能够自动完成重复劳动，相对可以跨多个设备运行同一套任务；适合比较固定的业务
* 缺点：并不是所有操作都可以自动化，例如有安全保护防止自动化的界面，有特殊舒适，另外业务变化快的情况下自动化脚本维护成本会剧烈上升
* 建议：配合手工测试
* Appetizer提供了Python命令行工具，用于配合自动化脚本运行插桩包进行分析出报告
  * [插桩分析 命令行(Python) ](../integration/insights.py.html)
  * [Appetizer 整合梳理](https://testerhome.com/topics/10290)

## UI压力测试Maxim
[Maxim](https://github.com/zhangzhao4444/Maxim) 是一个基于uiautomator的快速UI界面压力测试工具，可以全自动解析界面控件树，并按照一定遍历规则进行点击、滑动、输入等操作；可通过Appetizer 主导航->APP测试->UI压力测试来使用；
![](https://testerhome.com/uploads/photo/2018/8028ddbc-38c2-427b-8864-336e9316ed52.gif!large)
* 优点：稳定可靠(Android 5-7)，速度快，全自动，可配置
* 缺点：对复杂业务（需要特殊输入的不合适），APP需要配置跳过难以自动化的部分，例如登录、表单等

Appetizer简化了配置 Maxim 命令行的工作； Appetizer 提供了以下便利：
* 自动产生命令行固定部分，不再需要检查打字打错
* 图形化支持所有 Maxim 参数，选择特定模式显示配置仅该模式支持的子参数；保证参数配置正确，方便好用
* 在图形化界面中直接编辑各种配置文件，不再需要手工push到设备配置名字，开始测试时 Appetizer 自动产生配置文件并push（见下图）
* 非常方便的黑白名单界面，轻松点击指定黑白Activity不再需要记忆长长的Activity全名
* 快捷批量操作，批量下载截图，批量删除；问题一键上报作者
![](maxim-overview.png)

|  一应俱全的Maxim配置  | 黑白名单 | 问题上报作者，下载结果 | 
|:-----------------:| :---------------: | :--: |
|  ![](maxim-config.png) |![](maxim-whitelist.png)| ![](maxim-download-results.png)|

## UI遍历测试 AppCrawler
[AppCrawler](https://testerhome.com/topics/8343)是一个基于Appium的自动化遍历工具，可以全自动解析界面控件树，通过配置进行遍历以及比较复杂的交互方案，适用Android/iOS；可通过Appetizer 主导航->APP测试->UI自动遍历来使用
![](https://testerhome.com/uploads/photo/2017/25cdb6a8-d6cb-4891-b0ff-5ccd7c3005c8.png!large)
* 优点：可用于Android/iOS，设计合理的遍历规则的情况下可以达到比较好的自动化效果
* 缺点：继承了Appium的所有Bug，偶尔不稳定，交互速度慢等；

Appetizer简化了配置 AppCrawler 的工作； Appetizer 提供了以下便利：
* 所见即所得YML配置文件编辑器，不用跑就知道配置格式对不对
* 图形化支持所有 AppCrawler 参数
* 配置文件自动产生，自动更新，保证每次都是最新版本，不用特别指定路径

```
----------------
AppCrawler 2.0.0
app爬虫, 用于自动遍历测试. 支持Android和iOS, 支持真机和模拟器
帮助文档: http://seveniruby.gitbooks.io/appcrawler
移动测试技术交流: https://testerhome.com
感谢: 晓光 泉龙 杨榕 恒温 mikezhou yaming116
感谢提供商业支持的优秀公司: Keep
--------------------------------


Usage: appcrawler [options]

  -a, --app <value>        Android或者iOS的文件地址, 可以是网络地址, 赋值给appium的app选项
  -c, --conf <value>       配置文件地址
  -p, --platform <value>   平台类型android或者ios, 默认会根据app后缀名自动判断
  -t, --maxTime <value>    最大运行时间. 单位为秒. 超过此值会退出. 默认最长运行3个小时
  -u, --appium <value>     appium的url地址
  -o, --output <value>     遍历结果的保存目录. 里面会存放遍历生成的截图, 思维导图和日志
  --capability k1=v1,k2=v2...
                           appium capability选项, 这个参数会覆盖-c指定的配置模板参数, 用于在模板配置之上的参数微调
  -r, --report <value>     输出html和xml报告
  --template <value>       输出代码模板
  --master <value>         master的diff.yml文件地址
  --candidate <value>      candidate环境的diff.yml文件
  --diff                   执行diff对比
  -vv, --verbose           是否展示更多debug信息
  --help
示例
appcrawler -a xueqiu.apk
appcrawler -a xueqiu.apk --capability noReset=true
appcrawler -c conf/xueqiu.json -p android -o result/
appcrawler -c xueqiu.json --capability udid=[你的udid] -a Snowball.app
appcrawler -c xueqiu.json -a Snowball.app -u 4730
appcrawler -c xueqiu.json -a Snowball.app -u http://127.0.0.1:4730/wd/hub

#启动已经安装过的app
appcrawler --capability appPackage=com.xueqiu.android,appActivity=.welcomeActivity

#从已经结束的结果中重新生成报告
appcrawler --report result/

#新老版本对比
appcrawler --candidate result/ --master pre/ --report ./

#自动生成Page Object代码模板文件
appcrawler --template PageObjectDemo.ssp --output result/

#根据wda的inspector生成测试用例代码
appcrawler --template PageObjectDemo.ssp -u http://localhost:8100
```
