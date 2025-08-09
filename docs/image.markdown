# 类：`turquoise::Image`
* 图像资源类
* 头文件 `turquoise/image.h`
## 相关
```cpp
struct Image {
    unsigned char* pixels = nullptr;
    int width;
    int height;
    int channel;

    Image() = default;
    Image(Image& other);
    Image(Image&& other);
    Image(const char* path, Log& log, Log& errorLog);
    Image(const char* path);
    Image(const char* path, int channelNum, Log& log, Log& errorLog);
    Image(const char* path, int channelNum);
    ~Image();

    void operator= (Image& other);
    void operator= (Image other);

    void clear();
    inline uint64_t size() const {
        return width * height * 4;
    }
};
```
## 生命周期
> 根据RAII申请和释放资源。

## 提示
> 不可复制，只能移动。