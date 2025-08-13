# 类型 `turquoise::OStr`
* **定义**
  ```cpp
  template<class T>
  using OStr = const T*;
  ```
* > 原始串
# 类： `turquoise::Str`
* > 记录特定数据类型的一组串
* > 位于`turquoise/str.hpp`
## 模板参数: `<class T, class Allocator = std::allocator<T>>`
* > `class T`: 特定数据类型
* > `Allocator`: (可选): 满足c++标准的allocator， 默认使用std::allocator
## 构造
* > `Str(OStr<T>)`: 用常数组构造, 最后一位是NULL
* >`Str(std::initalizer_list<OStr<T>>)`: 拼接一组常数组（同上）构造
* > `Str(int)`： 用长度构造
## 方法
* > `int length()`： 获取长度
* > `T* data`: 获取原始指针
* > `加法运算符+`：返回两者拼接结果，按先后顺序
* > `赋值运算符=(左值)`：拷贝
* > `[]`: 获取指定下标位置的元素引用
## 示例
```cpp
// 示例： 拼接出Hello world!
#include "turquoise/turquoise.h"
#include <iostream>
using namespace turquoise;

int main() {
    //方案一
    Str<char> string1("Hello");
    Str<char> string2("world");
    Str<char> string3 = string1 + " " + string2 + "!";

    for (int i = 0; i < string3.length(); i++) {
        std::cout << string3[i];
    }

    std::cout << std::endl;
    //方案二
    Str<char> string4({"Hello", " ", "world", "!"});
    
    for (int i = 0; i < string4.length(); i++) {
        std::cout << string4[i];
    }

    return 0;
}
```
### 输出
```
Hello world!
Hello world!
```
### 提示
* 出于性能考虑(避免内存碎片)，建议更多的使用方案二，方案一主要适用于需要更多灵活性场景。
* 字符串处理和简单操作场景建议使用这个类而非`std::string`，as we all know, `std::string` 并不靠谱。