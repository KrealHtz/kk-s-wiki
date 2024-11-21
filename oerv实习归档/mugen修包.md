![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411212018660.png)
从报错日志可以看keras已经安装完成，但是它的使用依赖于tensorflow库，tensorflow还没有支持riscv架构的预编译版本。之前已有issue指出 LLVM_2409 RISC-V llvm 版本缺少软件包python3-tensorflow。
 [LLVM平行宇宙24.09 缺少软件包 python3-tensorflow](https://gitee.com/openeuler/RISC-V/issues/IARG19?from=project-issue&search_text=tensorflow)。



![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411212036406.png)
从报错日志可以看出缺少pki-tools软件包






一、缺陷信息  
mugen测试套中的测试用例deepin-devicemanager执行失败，原因是缺少zeromq软件包

内核信息：  
Linux openeuler-riscv64 6.6.0-35.0.0.44.mg2409.riscv64 #1 SMP Thu Aug 15 02:08:10 UTC 2024 riscv64 riscv64 riscv64 GNU/Linux

【环境信息】  
软件信息  
[https://repo.tarsier-infra.isrc.ac.cn/openEuler-RISC-V/testing/20240826/v0.1/QEMU/openEuler-Mega_24.09-V1-base-qemu-testing.qcow2.zst](https://gitee.com/link?target=https%3A%2F%2Frepo.tarsier-infra.isrc.ac.cn%2FopenEuler-RISC-V%2Ftesting%2F20240826%2Fv0.1%2FQEMU%2FopenEuler-Mega_24.09-V1-base-qemu-testing.qcow2.zst)

【问题复现步骤】  
执行命令yum install deepin-devicemanager  
【实际结果】  
![输入图片说明](https://foruda.gitee.com/images/1730391916095431991/1544dfc1_9523745.png "屏幕截图")



