title: 录制重放 Python 封装
---
`replaykit.py`是对`replaykit`的Python封装。 两个项目都是跨平台的，可以在 Windows 7+, MacOS Mavericks+ 和 Linux x64上使用。`replaykit.py`支持Python 2.7, 3.3-3.6.

## 安装要求
- 下载 [replaykit](https://github.com/appetizerio/replaykit).
- 安装Android SDK自带的adb命令行工具，确保adb的路径在`PATH`环境变量中
- 将需要调试的设备通过USB连接到开发机，并开启ADB调试（设置->开发者选项）。确保命令行执行`adb devices`可以看到连接的设备。如果是小米手机，请打开开发者选项中的“ADB调试（安全特性）”。

## 安装
通过 `pip`支持Python 2.7, 3.x
```
pip install appetizer
```

## 使用
所有的SDK API都通过Python标准的docstring提供详细的功能描述、参数类型、返回值类型等文档内容。开始使用前首先需要初始化一个 `Appetizer` 对象，后继所有的功能由这个对象提供。初始化需要提供`replaykit`的路径，如下：
```python
from appetizer import Appetizer
appetizer = Appetizer(toolkit='path/to/the/replaykit/executable') # e.g., './replaykit/linux/appetizer' for linux, or './replaykit/win32/appetizer.exe' for windows
```

### 多设备控制
Appetizer 可以同时在多个设备上执行同一个命令，类似一个adb的多机版本:
```python
# 列举所有已经连接的可以使用的设备
print (appetizer.devices.list())
# 获取一个设备的屏幕截图
appetizer.devices.take_screenshot("path/to/save/jpg/file", "serial_number")
# 安装APK文件到多个设备
appetizer.devices.control(["serialno1", "serialno2"], 'install', ['path/to/APK/file'])
# 启动一个APP
appetizer.devices.control(["serialno1", "serialno2"], 'launch_pkg', ['com.example.app'])
# 在多个设备上同时执行一个shell命令并获取返回信息
print(appetizer.devices.control(["serialno1", "serialno2"], 'shell', ['ls | grep system'])
# 镜像控制：将一个设备上的触摸屏输入实时投射到多个从属设备上
task = appetizer.devices.mirror("master_serialno", ["slave_serialno1", "slave_serialno2"])
# task对象代表了正在执行的镜像控制任务
time.sleep(5)
task.stop()
```

### Trace管理
Trace 管理功能能够录制并重放触摸屏输入事件
```python
# 从一个设备录制触屏输入，trace文件是二进制的，录制会新建一个线程
record_task = appetizer.trace.record("path/to/trace/file", "device_serial_number")
time.sleep(10)  # 主线程会独立于录制线程运行，可以选择等待一段时间或者做其他事情
record_task.stop()  # stop通知录制线程完成录制
# 向多个设备重放录制的触屏输入trace，要求多个设备与原设备有同样的纵宽比，重放会新建一个线程
replay_task = appetizer.trace.replay("path/to/trace/file", ['serialno1', 'serialno2'])
replay_task.wait() # 主线程等待重放线程完成
# 获取trace的信息，例如长度、纵宽比等
print(appetizer.trace.get_info("path/to/trace/file"))
```

### ADB server 控制
ADB是Appetizer与多个设备通讯的桥梁。adb命令行工具必须正确安装，并且adb server必须正常运行在开发机上。以下代码段展示了如何用本Python SDK控制本机的adb server:
```python
# 检测本机以及安装的adb命令行工具（通过PATH环境变量）
print(appetizer.adb.detect_adb())
# 启动本机的 adb server
appetizer.adb.start_server()
# 停止本机的 adb server
appetizer.adb.kill_server()
# 检查本机adb server的运行情况
print(appetizer.adb.check_server())
```

## Troubleshooting
