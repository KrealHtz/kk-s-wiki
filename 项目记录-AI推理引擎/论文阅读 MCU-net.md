MCUNet，基于TinyNAS和TinyEngine提出的可以运行在MCU上面的网络。
## 什么是Tiny Machine Learning
  
Tiny Machine Learning（TinyML），中文可以翻译成微型机器学习。正如名字表达的一样，能够在IoT如MCU等资源受限（如内存、计算能力、能源）的设备上运行的机器学习模型和算法，最大的特点就是“小”。区别于现在流行的大模型，TinyML的尺寸远远小于一般的机器学习模型，更加远远小于大模型。虽然并不是当前GPT这类的大模型，但是TinyML的设计和部署思路，在大模型时代依然值得研究学习。今天的“大“模型也许也会成为未来的“小”模型。  
TinyML有着很多的优点：低功耗，小型化，实时处理，性价比更高，并且可以进行隐私的保护。现在很多的物联网设备上都已经用上了这项技术，例如智能家居（自动调节温度的恒温器，智能灯光系统），健康监测（检测心率，睡眠质量）等等
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202404091533418.png)

### Tiny ML的挑战
  
Tiny ML最大的问题是资源的限制：**没有DRAM，没有操作系统，受限的SRAM**（一般小于256kB，并且FLASH是只读的）。考虑到机器学习网络一般涉及大量的矩阵运算，以及参数的存储，如何在这种资源很有限的情况下进行推理或训练，是一个很大的问题。  
训练对于推理来说，又是更大的挑战，因为训练里有大量的中间参数需要存储，而这也就意味着需要更大的内存。  
而通常的MCU，例如，一款热门的ARM Cortex-M7微控制器，STM32F746，只有320 KB的静态随机存储器（SRAM）和1 MB的闪存。在深度学习场景中，SRAM限制了activation的大小（读和写），而闪存限制了模型的大小（只读）。此外，STM32F746的处理器时钟速度为216 MHz，比笔记本电脑低10-20倍。
### Tiny ML 算法方案的进展

常用的压缩神经网络的方法有：剪枝（pruning），量化（quantization）除此之外，张量分解（Tensor Decomposition）和知识蒸馏（Knowledge distillation）也是很好的压缩网络的方法。  
近期，被关注最多的方法是**神经架构搜索（Neural Architecture Search，NAS）**:一种自动化机器学习技术，旨在自动找到适用于特定任务的神经网络架构，而无需手动设计或调整网络结构。  
NAS高度依赖搜索空间，对于MCU来说，由于存储空间的限制，标准的模型设计和恰当的空间搜索是很缺乏的。  
MCUNetV2表明不均衡的内存分布是在CNN网络设计里面的最主要的内存的瓶颈，最先的几个block里面的内存使用比剩余网络要大出一个数量级，所以这篇工作的重点放在receptive field redistribution。  

**系统方案**
流行的训练框架有：**PyTorch, TensorFlow, MXNet,** and **JAX**。不过这些框架都依赖于主机的语言，例如python和各种各样的runtime systems 。很明显用在边缘端设备上是没有必要也不兼容的  
也有例如 **TVM，TF-Lite , MNN, NCNN, TensorRT,** and **OpenVino** 这些可以用在在手机端的框架，但是这些框架在IoT设备上也是不适应的。  
也有很多提出支持ARM Cortex-M等MCU的框架，例如CMSIS-NN，X-Cube-AI, CMix-NN， TinyEngine， TF-Lite Micro （第一个支持在bare-metal控制器上面运行的框架）等等
