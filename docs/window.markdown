# 命名空间 `turquoise::key`
* 按键相关
* 头文件 `turquoise/window.h`
## 枚举类 `turquoise::key::Type`
> 按键种类
## 枚举类 `turquoise::key::State`
> 按键状态
## 类 `turquoise::Window::API`
* 初始化窗口类相关api。
* 应是单例，程序中只需创建一次。
### 构造函数
* > `API();`
* > `API(Log* APIErrorLog, Log& log, Log& errorLog);`
### 方法
* > `static std::vector<const char*> extensions();`：获得面向`turquoise::Instance`（vulkan instance）所需要的扩展信息。
# 类 `turquoise::Window`
* 窗口相关
* 头文件 `turquoise/window.h`
## 构造函数
* > `Window(core::_::Instance& instance, const int width, const int height, const char* title, Log& log, Log& errorLog);`
* > `Window(core::_::Instance& instance, const int width, const int height, const char* title);`
## 方法
* > `void getConfigs(core::_::physical_device_t device);` ：获得窗口最优（当前）配置
* > `static void pullEvents();`：拉取事件
* > `static void waitEvents();`：等待事件
* > `void close();`:关闭窗口
### 获取一系列窗口信息
* > `int height() const;`
* > `int width() const;`
* > `bool shouldClose() const;`
* > `core::_::Surface& surface() const;`
* > `core::_::format_t imageFormat() const;`
* > `core::_::color_space_khr_t imageColorSpace() const;`
* > `core::_::extent_2d_t imageExtent() const;`
* > `unsigned int minImageCount() const;`
* > `core::_::Surface::transform_flag_bits_khr_t currentTransform() const;`
* > `core::_::present_mode_khr_t presentMode() const;`
### 设置窗口行为
* > `void setKeyCallback(std::function<void(key::Type, key::State)> callback);`
* > `void setCloseCallback(std::function<void()> callback);`
* > `void setIcon(Image& image);`
## 示例
``` cpp
#include "turquoise/turquoise.h"
using namespace turquoise;

/* dir, err 是一组用于传入turquoise 构造器的日志对象
 * notice 用于程序自定义日志
 * winErr 用于Window::API报错信息
*/
Log dir("test.log", "DIR");
Log err("test.log", "ERR");
Log notice("test.log", "NTC");
Log winErr("test.log", "WER");

Window::API windowAPI(&winErr, dir, err);
core::_::Instance instance("Hello Turquoise", makeVersion(0,0,0), Window::API::extensions(), dir, err);
Window window(instance, 500, 500, "Hello Turquoise", dir, err);

int main() {
    notice.add("main start.");

    window.setCloseCallback( [](){
        notice.add("window close.");
    } );

    while (not window.shouldClose()) {
        Window::pullEvents();
    }

    notice.add("main end.");

    return 0;
}
```
## 结果 `test.log`
```log
2025/07/25 22:06:27.671 [DIR] Initialize window api successfully.
2025/07/25 22:06:27.705 [DIR] Create instance "Hello Turquoise" successfully.
2025/07/25 22:06:27.741 [DIR] Create window "Hello Turquoise" successfully.
2025/07/25 22:06:27.741 [NTC] main start.
2025/07/25 22:06:28.994 [NTC] window close.
2025/07/25 22:06:29.016 [NTC] main end.

```