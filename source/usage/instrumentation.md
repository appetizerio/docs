title: 插桩
---

# 插桩

## 什么是插桩
插桩技术可以自动修改APK中的DEX代码，对已有代码打点监控或者增加新的代码；插桩不需要源代码，可以处理混淆后的APK，可以广泛收集异常、ANR、网络请求、主线程卡顿、界面切换等信息，用于调试与测试。

## 浮动框功能
* 插桩提供 浮动框 功能，启用后再插桩APK，插桩包在启动后会有Appetizer浮动功能菜单。首先，在插桩前启用浮动框功能：
![](enable-floating-menu.png)
* 插桩APK，正常安装、授权，启动后如图，可以在设备上完成上传分析
![](floating-menu.png)

## APP权限要求
| 网络权限(声明即获取) | 外存读写权限(声明) | 外存权限(获取) | 可否插桩 | log位置 | 浮动框上传分析 | PC端上传分析 |
| :---: | :---: | :---: | :---: | --- | :---: | :---: |
| ✓ | ✓ |  ✓ | ✓ | `/sdcard/io.appetizer/<包名>.log` | ✓ | ✓ |
| X | ✓ |  ✓ | ✓ | `/sdcard/io.appetizer/<包名>.log` | X | ✓ |
| ✓ | ✓ |  X | ✓ | `/sdcard/io.appetizer/<包名>.log`(待获取外存权限后写出) | ✓ | ✓(待获取外存权限后才有数据) |
| X | ✓ |  X | ✓ | `/sdcard/io.appetizer/<包名>.log`(待获取外存权限后写出) | X | ✓(待获取外存权限后才有数据) |
| ✓ | X | X | ✓ | [app私有数据](https://developer.android.com/training/data-storage/files.html#WriteInternalStorage) | ✓ | X |
| X | X | X | X |  | X | X |

## 常见插桩问题排查
* 插桩失败 -> 查看列表中插桩任务的状态
  * 等待上传、服务器问题、未知问题 -> 请上报
  * APP没有外存读写权限 -> 请修改AndroidManifest增加 `WRITE_EXTERNAL_STORAGE` 以及 `READ_EXTERNAL_STORAGE` 权限
  * APP加固 -> 请插桩 debug 或者 release包，不能加固，详见 [混淆与加固](advanced/obfuscation.html)
  * APP已经插桩 -> 请插桩一个未插桩的APK，已插桩即可使用
* 插桩后启动闪退
  * 确认是否有签名检查机制，对插桩包进行重新签名后再试，详见 [混淆与加固](advanced/obfuscation.html)
  * 请查看 [插桩原理](advanced/under-the-hood-instrumentation.html)是否有相关情况，问题未解决 -> 请上报