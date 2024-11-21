![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411212018660.png)
从报错日志可以看keras已经安装完成，但是它的使用依赖于tensorflow库，tensorflow还没有支持riscv架构的预编译版本。之前已有issue指出 LLVM_2409 RISC-V llvm 版本缺少软件包python3-tensorflow。
 [LLVM平行宇宙24.09 缺少软件包 python3-tensorflow](https://gitee.com/openeuler/RISC-V/issues/IARG19?from=project-issue&search_text=tensorflow)。



![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411212036406.png)
从报错日志可以看出缺少pki-tools软件包
issue: https://gitee.com/openeuler/RISC-V/issues/IB6720







一、缺陷信息  
LLVM_2409 RISC-V llvm 版本缺少软件包pki-tools

内核信息：  
Linux openeuler-riscv64 6.6.0-38.0.0.46.oe2409.riscv64 #1 SMP PREEMPT Wed Aug 28 17:31:00 CST 2024 riscv64 riscv64 riscv64 GNU/Linux

【环境信息】  
硬件信息  
qemu-system-riscv64

软件信息  
[https://repo.tarsier-infra.isrc.ac.cn/openEuler-RISC-V/testing/20240826/v0.1/QEMU/](https://gitee.com/link?target=https%3A%2F%2Frepo.tarsier-infra.isrc.ac.cn%2FopenEuler-RISC-V%2Ftesting%2F20240826%2Fv0.1%2FQEMU%2F)

【问题复现步骤】  
在运行mugen测试套pki-core时测试无法通过  
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411212042735.png)

是由于oe24.09 riscv64 llvm源里缺少pki-tools这个软件包。  


