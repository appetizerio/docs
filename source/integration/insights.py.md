title: 插桩分析 命令行(Python)
---

# 插桩分析 命令行(Python) [![Build Status](https://travis-ci.org/appetizerio/insights.py.svg?branch=master)](https://travis-ci.org/appetizerio/insights.py) 

使用流程
* 将待测试的 apk 上传到服务端进行插桩
* 下载插桩后的 apk
* 安装插桩后的应用，授权，进行测试流程（自动化测试，人工测试都可以），log会存在手机本地
* 将设备通过USB连接到开发机，并使用本客户端将 log 上传至服务端进行分析
* 通过[Appetizer Desktop](https://appetizer.io) >= 1.3.0进行可视化查看报告

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


## 环境要求
* Python 2.7.x 3.3+
* node >= 0.10.4
* adb已经安装并已经添加到环境变量PATH中
* 支持Windows, MacOS, Linux
* APK文件名尽量用英文，APP本身状态、字符串等可以有中文

## 用法
### 安装依赖
``` Shell
python -m pip install -r requirements.txt
```
### 帮助
``` Shell
python insights.py -h
```
确认不会有错误即依赖安装正确

### login: 登录账号
``` Shell
python insights.py login username password
```
所有客户端操作均需要登录认证，执行登录后登录用token保存在当前 `.access_token` 文件。token默认60天有效，使用任意操作会自动续期。60天后token过期需要重新登录。

账号可在 [Appetizer.io](https://api.appetizer.io/user/register) 注册。

### 插桩 apk （需要node）
``` Shell
python insights.py process apk processed_apk
```

例如
``` Shell
python insights.py process my.apk my_processed.apk # 插桩 my.apk，完成后保存为 my_processed.apk
python insights.py process my.apk my_processed.apk --enable-inapp-menu # 插桩 my.apk，完成后保存为 my_processed.apk，打开浮动框

```

插桩需要上传、处理、下载，需要一定时间，依据网络情况与APK大小不同大致在1分钟-3分钟内，期间会有输出表示进展情况。

### ~安装插桩后的APK并授权（需要node, adb）~

** 新版本插桩包不需要授权，直接adb 安装即可 **
``` Shell
python insights.py install my_processed.apk -s serialno1 -s serialno2
```
* `my_processed.apk` 是插桩后的apk
* `-s`是可选参数， `serialno1` `serialno2` 之类的是设备的串号，通过 `adb devices` 获得，需要安装到多个设备可以用多个`-s`命令指定设备; 不提供任何`-s`命令时，默认认为只有一个设备并对该设备进行安装
* 安装后会自动授权log （小米无法自动化授权，建议在安装完成后授权读写SDCARD）


### 测试
Appetizer 质量监控客户端对测试没有特别限制，可以是简单的人工测试，也可以是复杂的回归测试，测试长度不限。APP只要开始运行，插桩代码即开始收集数据

### 上传log获取分析报告（需要node, adb）
``` Shell
python insights.py analyze my_processed.apk -s serialno1 -s serialno2 --clear
```
* `-s`是可选参数， `serialno1` `serialno2` 之类的是设备的串号，通过 `adb devices` 获得，需要分析多个设备上的log可以用多个`-s`命令指定设备; 不提供任何`-s`命令时，默认认为只有一个设备并对该设备进行分析
* `--clear`是可选参数，用于从设备下载log后将设备上log清空
* 分析成功后可以通过[Appetizer Desktop](https://appetizer.io) >= 1.3.0 查看
* 分析成功后，如果服务器可以导出报告，相应的导出报告下载路径会显示，例如：
``` Shell
waiting...... server is uploading the report
server has generated and uploaded the report
exported reports available at:
{
    "csv": "http://cache.appetizer.io/xxxxx.csv.zip",
    "json": "http://cache.appetizer.io/xxxxx.json.zip",
    "html": "http://cache.appetizer.io/xxxxx.html.zip",
}
```

### 强制清除log（需要node, adb）
``` Shell
python insights.py clearlog my_processed.apk -s serialno1 -s serialno2
```
将设备上有指定插桩后的APK的log清除
