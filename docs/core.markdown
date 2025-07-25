# 命名空间 `turquoise::core`
* `Turquoise` 基础组件
* 一些方便使用的预设
* `turquoise::core::_`中是底层组件（里接口）
* 头文件 `turquoise/core_back.h`
* 头文件 `turquoise/core_front.h`

## 里接口
### 概述
封装存放了vulkan提供的基础api, 结合表接口中提供的预设可以轻松编写
vulkan程序，与此同时不会污染命名空间。
### 句柄
每个句柄中存放代表vulkan对象的`entity`，一般还存放依赖的逻辑设备：
`device`。有默认构造，拷贝构造，移动构造，赋值重载运算符，和`void clear()`方法用以销毁句柄。值得注意的是，Turquoise里接口中的句柄类实际上无法被复制，只能被移动和引用，这保证了实际的vulkan对象的生命周期可以被掌握。
### 所有句柄类
* Instance
* PhysicalDevice
* LogicalDevice
* Surface
* Semaphore
* Fence
* Swapchain
* ShaderModule
* Sampler
* DescriptorSetLayout
* PipelineLayout
* DescriptorPool
* RenderPass
* GraphicsPipeline
* CommandPool
* DeviceMemory
* Buffer