# 类：`turquoise::Image`
* 图像资源类
* 头文件 `turquoise/image.h`
## 构造
``` cpp
Image() = default;
Image(const char* path, Log& log, Log& errorLog);
Image(const char* path);
Image(const char* path, int channelNum, Log& log, Log& errorLog);
Image(const char* path, int channelNum);
```
## 生命周期
> 根据RAII申请和释放资源。