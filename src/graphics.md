# 图形

## 有关的一些定义

- **定义：**
  - OpenGL: 一个跨平台、跨语言的图形API标准（specification)
  - GLX: OpenGL对X Window System的扩展
  - GLUT, GLFW: 为OpenGL窗口的工具库
  - XCB: 与libX11一样，是X Window System协议客户端API的实现库
- API标准：
  - OpenGL
  - DirectX
  - Vulkan
- 与OpenGL有关的库：
  - 上下文和窗口工具箱：
    - GLFW
    - GLUT
  - 实现：
    - Mesa
  - 扩展加载库：
    - GLEW
    - GLAD

> <pre>
> OpenGL is a cross-language and cross-platform graphic API specification
> GLX: OpenGL extension to the X Window System
> Vulkan: a low-overhead, cross-platform API, open standard for 3D graphics and computing
> GLUT: a library of utilities for OpenGL programs.
> GLFW: a lightweight utility library for use with OpenGL (stands for graphics library framework)
> XCB: a X11 protocal client-side implementation
>API standard:
> OpenGL
> DirectX
> Vulkan
> 
>Associated libraries:
> Context and window toolkits:
>  GLFW
>     GLUT
>    Implementation:
>  Mesa
>    Extension loading libraries:
>  GLEW
>     GLAD
>    </pre>

## OpenGL

首先要明确OpenGL的设计是基于状态的，利用函数设置状态，作出指令，让OpenGL执行。也正是因为这样，OpenGL编程也变得复杂，要考虑到各个方面各个状态，否则可能会得不到想要的结果。

### 使用的库

#### GLEW

GLEW是一个加载库，由于OpenGL是一个API标准，每个GPU驱动中OpenGL实现中的各函数入口位置也不一样，为了使已经编译好的程序有可种植性，需要在运行的时候动态寻找函数入口。

#### GLFW

GLFW是一个窗口工具库，并且提供了相关的上下文，利用GLFW可以进行窗口操作，以及事件（如鼠标、键盘）处理。

在之后的示例中，将使用GLFW和GLEW作为窗口工具库和外部加载库。