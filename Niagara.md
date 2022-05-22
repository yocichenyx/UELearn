# UE4 Niagara Overview
> Niagara是UE Cascade之后的下一代VFX系统，美术可以自己创建附加功能而不需要程序的帮助。
> [官方文档](https://docs.unrealengine.com/4.27/en-US/RenderingAndGraphics/Niagara/Overview/)
## 核心组件
- **System-系统**
  - 很多个发射器组成一个效果，而系统是发射器的容器
- **Emitter-发射器**
  - 发射器是模块的容器，作单一用途，但是可以复用。独特之处在于，可以在同一个发射器上用模块栈(多模块栈叠加)创建一个模拟，而用多种方式来渲染该模拟。
- **Modules-模块**
  - 模块是Niagara系统之基，相当于Cascade。它访问公共数据，封装行为，与其他模块叠加来完成功能。
  - 以HLSL构建，也可用可视化的节点构建。可以创建包含输入的函数，或者写入值或者参数字典。甚至可以在其Graph内用CustomHLSL节点来写HLSL。
- **Parameters-参数**
  - 参数是Niagara内数据的抽象，有四种类型
  - 参数类型：
    - **Primitive**:不同精度和通道宽度的数值数据
    - **Enum**:一组固定命名值之一，即枚举
    - **Struct**:Primitive和Enum的集合
    - **Data** Interface:由外部数据源(UE的其他模块或者外部程序)提供数据的函数
## 栈模型和模块分组
- Niagara的粒子模拟就像一个**自上而下的栈**模拟，按顺序执行可编程的代码块——模块。
- **每个模块都归属于组，组决定了模块的执行时间**
## Niagara VFX 工作流
- 创建发射器(各个Group的作用)
  - EmitterSpawn:   放置定义发射器出现时起效的模块
  - EmitterUpdate:  放置随时间影响发射器的模块
  - ParticleSpawn:  放置定义粒子从发射器生成时起效的模块
  - ParticleUpdate: 放置随时间影响粒子的模块
  - EventHandlers:  可以在定义某些数据的一个或多个发射器中创建生成事件，然后在其他发射器中监听该事件，进而触发响应行为。（**这个还不太懂，好像现在只能订阅固定的事件**）
- 创建系统
- 创建模块
  - 模块使用HLSL，逻辑流程：
  - Graph逻辑 -> HLSL交叉编译器(生成目标平台可执行代码的编译器))
    - HLSL交叉编译器 -> Target GPU Platform
    - HLSL交叉编译器 -> SMID Optimized Virtual Machine(CPU)
## Niagara范式
- 继承：分层继承，可复用，可重写
- 动态输入：减少模块提高性能 **为啥？**
- Micro Expressions：对于不需要新模块的小型一次性特性非常有效。**为啥？**
- 事件：粒子发射器系统之间通信的方式 **系统间也能通信？**
- 数据接口：允许访问任意数据的可扩展接口
- Houdini：可将Houdini中的数据导出为CSV，然后将其导入到Niagara
## 为什么要使用Niagara
- 属性开放(可由外部传入控制其中的关键属性，来更新效果)
- 可以嵌入逻辑(可以自定义Modulel逻辑，甚至可以在其中写HLSL代码)
