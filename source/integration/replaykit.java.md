title: 录制重放 Java 封装
---

# 录制重放 Java 封装
https://github.com/appetizerio/replaykit.java

## 安装要求
- 下载 [replaykit](https://github.com/appetizerio/replaykit).
- 安装Android SDK自带的adb命令行工具，确保adb的路径在`PATH`环境变量中
- 将需要调试的设备通过USB连接到开发机，并开启ADB调试（设置->开发者选项）。确保命令行执行`adb devices`可以看到连接的设备。如果是小米手机，请打开开发者选项中的“ADB调试（安全特性）”。

```java
import io.appetizerio.ReplayKit;

ReplayKit replaykit = new ReplayKit("/home/ken/replaykit/darwin/");
replaykit.Trace().getInfo("mytrace.trace");
```
