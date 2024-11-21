![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411212018660.png)
从报错日志可以看keras已经安装完成，但是它的使用依赖于tensorflow库，tensorflow还没有支持riscv架构的预编译版本。之前已有issue指出 LLVM_2409 RISC-V llvm 版本缺少软件包python3-tensorflow。
 [LLVM平行宇宙24.09 缺少软件包 python3-tensorflow](https://gitee.com/openeuler/RISC-V/issues/IARG19?from=project-issue&search_text=tensorflow)。



![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411212036406.png)
	co