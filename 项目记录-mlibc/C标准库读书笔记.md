为了区分标准库和自己写的头文件，在使用#include指令时最好进行区分：用双引号包含自己所写的头文件，而不是使用尖括号。只有引用`标准头文件`才使用尖括号。例如：
```
#include <stdio.h>
#include "plot.h"
```
---
`编程风格：`
库中的每个可见函数都占据着单独的c源文件。例如strlen就在strlen.c中。
头文件的实现：
1.幂等性
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202404072100160.png)
