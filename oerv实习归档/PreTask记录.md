## 任务一
- 任务一：通过 QEMU 仿真 RISC-V 环境并启动 openEuler RISC-V 系统，设法输出 neofetch 结果并截图提交
在进行之前的任务的时候已经安装了qemu，所以这里就直接从neofetch安装开始：
直接使用命令安装
```
dnf install neofetch
```
之后进行测试输入neofetch输出结果如下图
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411011517583.png)

## 任务二
- 任务二：在 openEuler RISC-V 系统上通过 obs 命令行工具 osc，从源代码构建 RISC-V 版本的 rpm 包，比如 `pcre2`。（提示首先需要在 openEuler的 OBS[什么是 OBS？](https://github.com/openeuler-riscv/oerv-team/blob/main/Intern/FAQ.md#%E4%BB%80%E4%B9%88%E6%98%AF-obs) 上注册账号 [https://build.openeuler.openatom.cn/project/show/openEuler:Mainline:RISC-V](https://build.openeuler.openatom.cn/project/show/openEuler:Mainline:RISC-V)）
--- 
**什么是obs？** Open Build Service（OBS）是一种用于构建、发布和维护软件包的开源工具，广泛应用于Linux系统的分发和软件打包。它由SUSE公司开发，旨在简化软件包的创建和管理，并支持多种Linux发行版（如openSUSE、Fedora、Debian等）和架构（如x86_64、ARM、PowerPC等）。OBS通过集中式的构建系统，允许开发者在一个平台上为多个目标系统生成软件包，便于跨平台软件的发布和维护。

注册obs账号之后，使用`dnf install osc build`安装osc命令行之后修改~/.config/osc/oscrc

```cobol
[general]
apiurl = https://build.tarsier-infra.isrc.ac.cn/
no_verify = 1
 
[https://https://build.tarsier-infra.isrc.ac.cn/]
user=kkhtz
pass=xxxxxx
```
安装配置工作完成之后，就可以开始构建了。
```
# 首先通过命令将obs平台上的分支拉下来
osc co openEuler:Mainline/pcre2
# 然后将远程包和其他相关文件一起拉下来
osc up -S # pcre2目录下
rm -f _service;for file in `ls`;do new_file=${file##*:};mv $file $new_file;done
# 开始构建rpm包
osc build standard_riscv64_gcc riscv64
```
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411011517168.png)

参考
[# oerv-pretask（二）在 openEuler RISC-V 系统上通过 obs 命令行工具 osc，从源代码构建 RISC-V 版本的 rpm 包，比如 coreutils](https://blog.csdn.net/weixin_52230326/article/details/135311782)
## 任务三：尝试使用 qemu user & nspawn 或者 docker 加速完成任务二
首先尝试使用qemu user & nspawn加速完成
- 安装qemu
- 安装osc
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411021256588.png)
	在后续的折腾中发现由于使用的是macos，在本机直接构建存在的问题太多了，遂考虑在电脑上搭建虚拟机完成，也发现了之前有同学使用macos进行任务时踩到的很多坑[# 实习生唐文荟+创建邮箱](https://github.com/openeuler-riscv/oerv-team/pull/271)
但是在fedora上构建任务三时 vm_type = chroot时报错
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411031512222.png)
vm-type = nspawn时报错
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411031513381.png)
这些问题好像是因为arm芯片的原因？找了好久都没找到解决办法。遂翻出笔记本老老实实在x86上做。
重新完成前面三个任务。使用fedora完成。
任务务一：
![Uploading file...drxcg]()


任务二：
![4e4777f6715ec82bf5b67dcdc220b1c1.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202411032320821.png)

任务三：
