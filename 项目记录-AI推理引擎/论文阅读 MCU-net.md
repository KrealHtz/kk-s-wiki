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
## MCU-Net
MCUNet：一种基于系统-算法协同设计框架，用于神经网络在MCU上的部署，它结合了神经网络架构优化（TinyNAS）和推理调度（TinyEngine）。
- **传统的神经网络架构搜索（NAS）**：在已有的深度学习库上搜索神经网络模型。这种方法首先使用NAS找到一个神经网络模型，然后在某个深度学习库（如TensorFlow或PyTorch）上实现这个模型。
- **调整深度学习库给定的神经网络模型**：针对一个已有的神经网络模型，调整深度学习库以达到更优的推理速度。这里，已有的模型和库之间的关系是相互调整和优化的。
- **MCUNet的系统-算法协同设计**：TinyNAS和TinyEngine结合使用，TinyNAS负责神经网络架构的搜索，而TinyEngine负责推理调度的优化。这种协同设计允许更有效地利用微控制器的有限资源，允许更大的设计空间进行架构搜索。与仅使用现有框架或库进行模型搜索相比，这种方法更有可能找到高准确率的模型。
### Tiny NAS
  
TinyNAS是一种**两阶段**的神经网络架构搜索方法。它首先优化搜索空间以适应微型和多样化的资源限制，然后在优化后的空间内进行神经网络架构搜索。通过优化搜索空间，显著提高了最终模型的准确度。

- 自动化搜索空间优化：TinyNAS提出了一种低成本的自动化搜索空间优化方法，通过分析满足模型的计算分布来实现。为了适应不同微控制器的微小和多样化资源限制，TinyNAS调整了输入分辨率和移动搜索空间的宽度乘数。它从输入分辨率R = {48, 64, 80, …, 192, 208, 224}和宽度乘数W = {0.2, 0.3, 0.4, …, 1.0}中选择，以涵盖广泛的资源限制范围。这导致了12 × 9 = 108种可能的搜索空间配置S = W × R。每个搜索空间配置包含3.3 × 10^25种可能的子网络。目标是找到最佳的搜索空间配置S*。
> **Resolution Spanning R**：这个参数集合定义了输入图像的分辨率，即网络输入层接受的图像尺寸。在这个上下文中，R的值从48到224，包含了一系列可能的分辨率（例如，48x48, 64x64, 80x80, ..., 192x192, 208x208, 224x224像素）。不同的输入分辨率会影响模型的大小和计算量，因为输入图像的尺寸直接关联到网络第一层的输入节点数。  
> **Width Multiplier W**：这是一个缩放因子，用于根据比例调整网络每层的通道数（即宽度）。例如，如果一个网络层原来有100个通道，宽度乘数为0.2，则调整后的网络层将有20个通道。这个参数可以在不显著改变网络结构的前提下调整模型的复杂性和计算量。宽度乘数越小，生成的模型越“窄”，即每层的通道数越少，模型的计算量和参数量也越小。

在寻找最佳搜索空间配置S*的过程中，TinyNAS采用了一种高效的方法，而不是直接在每个搜索空间上进行神经网络架构搜索并比较结果，因为那样的计算量将会非常巨大。

TinyNAS通过随机抽样m个网络，从搜索空间中比较满足条件的网络分布来评估搜索空间的质量。这种方法不是收集每个满足条件网络准确率的累积分布函数（CDF），因为这会因为巨大的训练量而计算量很大。相反，它只收集FLOPs（浮点操作次数）的CDF。这种方法的直觉是，**在同一模型家族内，准确率通常与计算量正相关。计算量较大的模型具有更大的容量，更有可能实现更高的准确率。**

> **FLOPs（浮点操作次**代表了执行神经网络一次前向传播所需要的浮点运算次数。它是衡量网络计算复杂度的一个指标。比如，一个网络有两层，每层做100次乘法和100次加法，那么这个网络的FLOPs就是400（因为乘法和加法都是浮点运算）。  
> **CDF（累积分布函数）**用来描述一个随机变量落在某个范围内的概率。在NAS的上下文中，CDF可以用来表示所有网络中有多少比例的网络达到或低于特定的FLOPs数。假设我们有一个简单的例子，我们有5个网络，它们的FLOPs分别是10，20，30，40，50。如果我们绘制它们FLOPs的CDF，那么在FLOPs为30时的CDF值是60%，意味着60%的网络有30或更少的FLOPs。

  
以在STM32F746上为ImageNet-100（从原始ImageNet中取出的一个100类分类任务）寻找最佳搜索空间为例，TinyNAS展示了前10个搜索空间配置的FLOPs分布CDF（见图5(b)）。在寻找最佳搜索空间S*的过程中，TinyNAS还考虑了内存需求和最佳调度，这是通过TinyEngine来实现的。只有那些满足内存要求并在TinyEngine最佳调度下有效的模型被保留。

例如，在ImageNet-100数据集上的实验结果表明，使用平均FLOPs为52.0M的实线红色空间，与使用平均FLOPs为46.9M的实线绿色空间相比，准确率提高了2.3%。这表明了自动化搜索空间优化的有效性。

通过自动化搜索空间优化和考虑内存需求及调度策略，TinyNAS能够更有效地为特定任务和硬件约束找到最适合的神经网络架构。这种方法不仅提高了模型的准确率，还确保了模型能够在资源受限的微控制器上运行，优化了推理性能和内存使用。
### TinyEngine：A Memory-Efficient Inference Library
正如名字所示，TinyEngine是一个存储空间有效的深度学习框架（libraries），最大的特点是相比于其他的库，所占用的内存更小，使得该库对于MCU来说非常的有效。  

- 代码编译 （Code Generation）：TinyEngine和其他的如TF-Lite Micro，CMSIS-NN基于解释器（Interpreter-based）的编译器，是基于生成器（Generator-based）的编译器。使得运行的时间更短，并且能够降低内存的使用。同时TinyEngine生成的二进制文件更小，也使得其内存更加的有效。
- 原地深度卷积（In-Place Depth-Wise convolution）：支持原地深度卷积操作，能进一步减少峰值内存使用。这种方法允许一旦某通道计算完成，它的输入激活就可以被覆盖并用于存储另一个通道的输出激活，这样就可以原地更新深度卷积的激活。

> 深度卷积是一种特殊的卷积操作，它与标准卷积不同。在深度卷积中，卷积操作是**分别在每个输入通道上独立进行的，而不是跨通道进行滤波**。这意味着每个滤波器只作用于单个输入通道，而不是像传统卷积那样，一个滤波器作用于所有输入通道。原地操作是指**直接在输入数据上修改数据**，而不是创建新的内存空间来存储输出结果。原地深度卷积意味着一旦某个通道的计算完成，它的输入激活（输入数据）就可以被覆盖，用来存储另一个通道的输出激活。这样可以节省内存，因为不需要额外的内存空间来存放输出结果。

- 基于补丁的推理（Patch-based Inference）：它通过逐块处理内存密集的初始阶段，每次只处理输入图像的一个小空间区域，从而大大减少了峰值内存。

> 核心思想是将大型图像或数据分成较小的区块（补丁），在每个补丁上独立进行推理计算，而不是在整个图像或数据上一次性进行。这样做可以大幅减少同时需要存储在内存中的数据量，因而减少了内存的峰值使用。  

- 重新分配感受野（Receptive Field, RF）：MCUNet提出通过重新分配CNN的感受野来减少计算开销，基本思路是：减少基于补丁的初始阶段的感受野，这有助于减少每个输入补丁的大小和重复计算。增加后续阶段的感受野，以补偿由于初始阶段感受野减小而可能造成的性能下降（例如，大对象检测性能可能会降低）。

### Co-Design: Joint Neural Architecture and Inference Scheduling Search

  
模型架构和推理引擎的优化算法是紧密耦合的。例如，重新分配感受野允许我们在最小的计算/延迟开销下享受内存减少的好处，这为设计主干架构提供了更大的自由度（例如，我们现在可以使用更大的输入分辨率）。为了探索如此大的设计空间，**MCUNet以自动化的方式联合优化神经网络架构和推理调度**。面对特定的数据集和硬件限制（SRAM限制、Flash限制、延迟限制等），我们的目标是在满足所有约束的同时达到最高的准确性。对于模型优化，它使用NAS找到一个好的候选网络架构；对于调度优化，它优化补丁数量p和执行基于补丁推理的块数n，以及TinyEngine中的其他设置。


"Million MAC/s" 是一个衡量计算性能的单位，其中 "MAC" 代表乘加运算（Multiply-Accumulate Operations），而 "s" 代表秒。这个单位用来描述处理器或者计算系统在每秒钟内能够执行多少个乘加运算。乘加运算是计算机中常见的基本操作，特别是在数字信号处理和高性能计算中，它涉及到一个乘法操作和一个加法操作的组合13。

在深度学习和其他科学计算领域，"Million MAC/s" 可以用来衡量模型的计算复杂度或者硬件的计算能力。例如，一个具有高 "Million MAC/s" 值的处理器能够更快地执行神经网络中的卷积层和其他涉及大量乘加运算的操作，从而在较短的时间内完成模型的训练和推理任务134。

此外，"Million MAC/s" 也可以用来比较不同硬件的性能，比如不同的GPU、DSP（数字信号处理器）或者专用的AI加速器。在实际应用中，这个指标可以帮助研究人员和工程师选择合适的硬件来满足特定的计算需求3。

需要注意的是，虽然 "Million MAC/s" 是一个重要的性能指标，但它并不是唯一的衡量标准。在评估计算系统时，还需要考虑其他因素，如功耗、内存带宽、软件优化等，这些都会影响到最终的计算效率和性能35。
