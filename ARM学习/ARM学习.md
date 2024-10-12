## ARM64异常处理
在ARM v8体系结构中，异常和中断都属于异常处理。
### 异常类型
异常分为，中断，中止，复位，系统调用等
`中断`分为普通中断请求（Interrupt Request IRQ）,和快速中断请求（Fast Interrupt Request, FIQ）,FIQ>IRQ 优先级。
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410122233798.png)
中止主要有`指令中止`（instruction abort）和数据中止（data abort）两种。它们通常是指访问内存地址时发生了错误（如缺页等），处理器内部的 MMU 捕获这些错误并且报告给处理器。
指令中止是指当处理器尝试执行某条指令时发生了错误，而数据中止是指使用加载或者存储指令读写外部存储单元时发生了错误。
复位操作是优先级最高的一种异常处理。用于让CPU复位引脚产生复位信号，让CPU进入复位状态，并重新启动，
### AArch64 的中断和异常处理

![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410102232935.png)


![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410102239620.png)

![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410102243245.png)

![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410102258691.png)



![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202410102246109.png)
