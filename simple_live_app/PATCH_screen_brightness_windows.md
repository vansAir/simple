# Simple Live Windows 亮度修改补丁

## 问题

`screen_brightness_windows` 插件默认开启 `auto_reset`，会在窗口焦点变化时调用 `SetMonitorBrightness` 强制修改物理显示器亮度。

## 补丁文件

`simple_live_app/lib/main.dart`

### 1. 添加 import（文件顶部 import 区域）

在已有的 import 块中添加：

```dart
import 'package:screen_brightness/screen_brightness.dart';
```

### 2. 在 `main()` 函数中 `initServices()` 之后插入

```dart
  // Windows平台禁用screen_brightness的自动亮度重置，避免插件强制修改显示器亮度
  if (Platform.isWindows) {
    try {
      await ScreenBrightness.instance.setAutoReset(false);
    } catch (e) {
      Log.logPrint(e);
    }
  }
```

插入位置：`await initServices();` 之后，`SystemChrome.setEnabledSystemUIMode(SystemUiMode.edgeToEdge);` 之前。

## 验证

构建 Windows 版本后，最小化/切换窗口焦点，显示器亮度不应再被强制修改。
