# 类: `turquoise::Log`
* 日志管理
* 线程安全
* 快速
* 头文件`turquoise/log.h`
## 构造
### `Log(const char* dst, const char* type, bool toConsole = false)`
  1. `const char* dst`: 目标日志文件
  2. `const char* type`: 代表日志类型
  3. `bool toConsole`：是否输出到控制台
## 方法
### `void add(const char* info)` 
   * 添加日志
   * `info`: 日志信息
## 测试（同步）
```cpp
// 添加一万条信息为50字节的日志
#include "turquoise.h"
using namespace turquoise;

int main() {
    Str<char> info("");
    for (int i = 0; i < 50; i++) {
        info = info + "-";
    }

    Log log("test.log", "TEST");
    for (int i = 0; i < 10000; i++) {
        log.add(info.data());
    }
    return 0;
}
```
### 结果
输出文件`test.log`
```log
2025/06/12 01:22:48.289 [TEST] --------------------------------------------------
2025/06/12 01:22:48.29 [TEST] --------------------------------------------------
2025/06/12 01:22:48.29 [TEST] --------------------------------------------------
2025/06/12 01:22:48.29 [TEST] --------------------------------------------------
2025/06/12 01:22:48.29 [TEST] --------------------------------------------------
...
2025/06/12 01:22:48.301 [TEST] --------------------------------------------------
2025/06/12 01:22:48.301 [TEST] --------------------------------------------------
2025/06/12 01:22:48.301 [TEST] --------------------------------------------------
2025/06/12 01:22:48.301 [TEST] --------------------------------------------------
2025/06/12 01:22:48.301 [TEST] --------------------------------------------------
2025/06/12 01:22:48.301 [TEST] --------------------------------------------------

```
## 测试（并行）
```cpp
#include "turquoise/turquoise.h"
#include <thread>
#include <windows.h>
using namespace turquoise;

int main() {
    Str<char> info("");
    for (int i = 0; i < 45; i++) {
        info = info + "-";
    }
    Str<char> info1 = info + "11111";
    Str<char> info2 = info + "22222";
    Str<char> info3 = info + "33333";
    Str<char> info4 = info + "44444";

    Log log("test.log", "TEST");

    std::thread( [&](){
        for (int i = 0; i < 2500; i++) {
            log.add(info1.data());
        }
    }).detach();
    std::thread( [&](){
        for (int i = 0; i < 2500; i++) {
            log.add(info2.data());
        }
    }).detach();
    std::thread( [&](){
        for (int i = 0; i < 2500; i++) {
            log.add(info3.data());
        }
    }).detach();
    std::thread( [&](){
        for (int i = 0; i < 2500; i++) {
            log.add(info4.data());
        }
    }).detach();

    Sleep(50);
    return 0;
}
```
### 结果 `test.log`
```log
2025/06/12 02:02:28.158 [TEST] ---------------------------------------------11111
2025/06/12 02:02:28.158 [TEST] ---------------------------------------------22222
2025/06/12 02:02:28.16 [TEST] ---------------------------------------------11111
2025/06/12 02:02:28.16 [TEST] ---------------------------------------------22222
2025/06/12 02:02:28.16 [TEST] ---------------------------------------------11111
2025/06/12 02:02:28.158 [TEST] ---------------------------------------------33333
2025/06/12 02:02:28.16 [TEST] ---------------------------------------------22222
2025/06/12 02:02:28.16 [TEST] ---------------------------------------------11111
2025/06/12 02:02:28.158 [TEST] ---------------------------------------------44444
2025/06/12 02:02:28.16 [TEST] ---------------------------------------------11111
...
2025/06/12 02:02:28.164 [TEST] ---------------------------------------------44444
2025/06/12 02:02:28.164 [TEST] ---------------------------------------------44444
2025/06/12 02:02:28.164 [TEST] ---------------------------------------------44444
2025/06/12 02:02:28.164 [TEST] ---------------------------------------------44444
2025/06/12 02:02:28.164 [TEST] ---------------------------------------------44444
2025/06/12 02:02:28.164 [TEST] ---------------------------------------------44444

```
## 提示
* 出于性能考虑，单条日志信息不建议超过64字节。
* 在头文件`config.h`中修改相关配置
``` cpp
7 #define TUEQUOISE_LOG_ALLOCATOR_BUFF_SIZE 64
8 #define TURQUOISE_LOG_FILE_BUFF_SIZE 8192
9 #define TURQUOISE_LOG_FILE_BUFF_NUMB 100
```