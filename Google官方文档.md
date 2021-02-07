### 1. Graphics概览

Android框架提供各种用于2D和3D图形渲染的API，可用于与制造商的图形驱动程序实现方法交互。

应用开发者可通过三种方式将图像绘制到屏幕上：使用Canvas、OpenGL ES和Vulkan。

#### 1.1 Android图形组件

无论使用哪种渲染API，最终一切内容都会渲染到``Surface``，Surface表示缓冲队列的生产方，缓冲队列通常会被SurfaceFlinger消耗。所有渲染的可见的Surface都被SurfaceFlinger合成到显示器上。

![ape-fwk-graphics](https://source.android.google.cn/devices/graphics/images/ape-fwk-graphics.png)

##### 图像流生产方

可以是生成图形缓冲区以供消耗的任何内容。

##### 图像流消耗方

最常见的消耗方是SurfaceFlinger，它会消耗当前可见的Surface，并使用窗口管理器中提供的信息将它们合成到显示器上。SurfaceFlinger使用OpenGL和Hardware Composer来合成一组Surface。

其他OpenGL ES应用也可以消耗图像流。

##### 硬件混合渲染器

SurfaceFlinger可以把某些合成工作委托给Hardware Composer，以分担OpenGL和GPU上的工作量。SurfaceFlinger只是充当另一个OpenGL ES客户端

##### Gralloc

图形内存分配器来分配图像生产方请求的内存。

#### 1.2 数据流

Android图形数据流的大致流程如下，左侧的对象是生成图形缓冲区的渲染器，如主屏幕、状态栏和系统界面；SurfaceFlinger是合成器，而硬件混合渲染器是制作器。

![graphics-pipeline](https://source.android.google.cn/devices/graphics/images/graphics-pipeline.png)

##### BufferQueue

调解缓冲区从生产方到消耗方的固定周期。

### 架构

#### 2.1 低级别组件

- BufferQueue和Gralloc
- SurfaceFlinger、Hardware Composer和虚拟显示屏
- Surface、Canvas和SurfaceHolder
- EGLSurface和OpenGL ES
- Vulkan

#### 2.2 高级别组件

- SurfaceView和GLSurfaceView

  SurfaceView的View组件由SurfaceFlinger合成，可以通过单独的线程/进程渲染，并与应用界面渲染隔离。

- SurfaceTexture

  SurfaceTexture将Surface与GLES纹理相结合来创建BufferQueue

- TextureView
