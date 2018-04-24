title: 录制重放
---
# 录制重放

[`replaykit`](https://github.com/appetizerio/replaykit) 提供了如下功能:
* 从单台设备录制屏幕触碰事件，在多台设备上重放（需要屏幕横宽比一致）
* 同时控制多台设备（例如, 运行一个 shell 命令，安装 apk 等）
* 将一台设备的屏幕触碰事件，镜像操作到多台设备上（需要屏幕横宽比一致）

`replaykit` 是一个命令行工具，能够运行在 Windows 7+，MacOS Mavericks+ 和 Linux 操作系统上。一同提供的还包括 [Python 封装](https://github.com/appetizerio/replaykit.py) 和 [Java 封装](https://github.com/appetizerio/replaykit.java)。 `replaykit` 要求 `adb` 已添加到 `PATH` 环境变量上.

欢迎提交关于 bug，功能和特性的 issues 以及 Pull requests。
