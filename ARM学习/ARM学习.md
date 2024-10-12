## ARM64异常处理
在ARM v8体系结构中，异常和中断都属于异常处理。
### 异常处理的基本概念
#### 异常类型
异常分为，中断，中止，复位，系统调用等
`中断`分为普通中断请求（Interrupt Request IRQ）,和快速中断请求（Fast Interrupt Request, FIQ）,FIQ>IRQ 优先级。
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410122233798.png)
中止主要有`指令中止`（instruction abort）和数据中止（data abort）两种。它们通常是指访问内存地址时发生了错误（如缺页等），处理器内部的 MMU 捕获这些错误并且报告给处理器。
指令中止是指当处理器尝试执行某条指令时发生了错误，而数据中止是指使用加载或者存储指令读写外部存储单元时发生了错误。
复位操作是优先级最高的一种异常处理。用于让CPU复位引脚产生复位信号，让CPU进入复位状态，并重新启动。
`系统调用` ARMv8 体系结构提供了 3 种软件产生的异常和 3 种系统调用。系统调用允许软件主动地通过特殊指令请求更高异常等级的程序所提供的服务。
SVC 指令：允许用户态应用程序请求操作系统内核的服务。

HVC 指令：允许客户操作系统（guest OS）请求虚拟机监控器（hypervisor）的服务。

SMC 指令：允许普通世界（normal world）中的程序请求安全监控器（secure monitor）的服务。
#### 异常等级
在操作系统里，处理器运行模式通常分成两种：一种是特权模式，另一种是非特权模式。
操作系统内核运行在特权模式，访问系统的所有资源；而应用程序运行在非特权模式，它不能访问系统的某些资源，因为它权限不够。除此之外，ARM64 处理器还支持虚拟化扩展以及安全模式的扩展。ARM64 处理器支持 4 种特权模式，这些特权模式在 ARMv8 体系结构手册里称为异常等级（Exception Level，EL）。
EL0 为非特权模式，用于运行应用程序。

EL1 为特权模式，用于运行操作系统内核。

EL2 用于运行虚拟化管理程序。

EL3 用于运行安全世界的管理程序。
#### 同步异常和异步异常
同步异常指的是处理器执行某条指令而导致的异常，往往需要在异常处理函数里处理该异常之后，处理器才可以继续执行。 例如，当数据中止时，我们知道发生数据异常的地址，需要在异常处理函数中修改这个地址。
常见的同步异常如下。
- 尝试访问一个异常等级不恰当的寄存器。
- 尝试执行关闭或者没有定义（undefined）的指令。
- 使用没有对齐的 SP。
- 尝试执行与 PC 指针没有对齐的指令。
- 软件产生的异常，如执行 SVC、HVC 或 SMC 指令。
- 地址翻译或者权限等导致的数据异常。
- 地址翻译或者权限等导致的指令异常。
- 调试导致的异常，如断点异常、观察点异常、软件单步异常等。

异步异常是指异常触发的原因与处理器当前正在执行的指令无关的异常，中断属于异步异常的一种。因此，指令异常和数据异常称为同步异常，而中断称为异步异常。
常见的异步异常包括物理中断和虚拟中断。

- 物理中断分为 3 种，分别是 SError、IRQ、FIQ。
- 虚拟中断分为 3 种，分别是 vSError、vIRQ、vFIQ

### AArch64 的中断和异常处理

![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410102232935.png)


![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410102239620.png)

![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410102243245.png)

![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410102258691.png)



![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410102246109.png)
