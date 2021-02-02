> 以下内容是《深入理解Android内核设计思想》一书第17章的笔记，详细内容请看原书。

### 1. OpenGL ES简介

OpenGL ES（OpenGL for Embedded Systems），专门面向嵌入式系统的OpenGL API子集，应用于PDA、手机等一系列电子产品中。不仅可以为Android开发者提供3D图形的实现，也广泛应用与2D的视频和图像处理。

1.x版本的OpenGL ES是“for fixed function hardware”，而2.x则是“for programmable hardware”，主要的差异就在于可编程性，扩展了图像硬件的功能以及灵活性，3.x版本则加入了更多新特性。

OpenGL ES也只支持3种基本的集合元素：点、线、三角形。

EGL：是图像渲染API（比如OpenGL ES）和本地窗口系统间的一层接口。主要用于提供如下功能：

- 创建rendering surfaces
- 创造图形环境（graphics context）
- 同步应用程序和本地平台渲染API
- 提供对显示设备的访问，以及对渲染配置的管理

### EGL接口

eglGetDisplay：由于EGL面对的是各种平台，因此需要一个机制来统一这些差异。该接口会得到一个EGLDisplay对象，该对象是一个与具体系统平台无关的对象，通过它来获取设备的Display。

eglInitialize：对EGL的内部数据进行初始值设定。

eglCreatePbufferSurface：eglCreateWindowSurface生成的Surface可用于在终端屏幕上显示，而eglCreatePbufferSurface生成的则是“离屏”渲染区。

eglCreateContext：由于OpenGL是一个状态机，对应的也要有EGL Context，用于为OpenGL的运行提供统一的环境

eglMakeCurrent：一个进程可能有多个Context，必须选择其中一个作为当前处理对象。该函数传入了两个EGLSurface，这两个都会被设置为当前Surface，所以我们一般都把这两个参数设为同一个Surface。
