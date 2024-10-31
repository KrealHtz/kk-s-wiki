## 任务一：通过 QEMU 仿真 RISC-V 环境并启动 openEuler RISC-V 系统，设法输出 neofetch 结果并截图提交
在进行之前的任务的时候已经安装了qemu，所以这里就直接从neofetch安装开始：
直接使用命令安装
```
dnf install neofetch
```
之后进行测试输入neofetch输出结果如下图
![image-20241029141324241](file:///Users/kkhtz/Library/Application%20Support/typora-user-images/image-20241029141324241.png?lastModify=1730374164)

## 任务二

- 任务二：在 openEuler RISC-V 系统上通过 obs 命令行工具 osc，从源代码构建 RISC-V 版本的 rpm 包，比如 `pcre2`。（提示首先需要在 openEuler的 OBS[什么是 OBS？](https://github.com/openeuler-riscv/oerv-team/blob/main/Intern/FAQ.md#%E4%BB%80%E4%B9%88%E6%98%AF-obs) 上注册账号 [https://build.openeuler.openatom.cn/project/show/openEuler:Mainline:RISC-V](https://build.openeuler.openatom.cn/project/show/openEuler:Mainline:RISC-V)）





- 任务三：尝试使用 qemu user & nspawn 或者 docker 加速完成任务二