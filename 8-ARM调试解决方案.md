# ARM调试解决方案

这段时间以来我一直在研究和调试相关的东西，最开始完全是基于x86做的，所以全是gdb和gdbserver。但arm却需要借助硬件调试。所以，就想看看现在国内都有什么著名的ARM开发工具和解决方案。价格从高低排：

**BDI1000/2000/3000**

> 目前我知道的最牛X的调试工具，可以调试ARM、MIPS、PPC、ColdFire、XScale等多种处理器。无需更换硬件，只需要买不同的软件授权就可以调试不同的CPU。JTAG下载速度可以上兆，以太网接口。因为太贵了（BDI2000好像要人民币50000吧），我没怎么研究它到底配合什么软件来调试，不过GDB它是肯定支持的，它一直是我心目中的神话啊。

**J-Link原版**

> J-Link是IAR公司为ARM开发的调试工具，支持RDI协议的调试工具，如Keil、ADS、IAR等；支持GDB调试；什么SWD之类的用得很少，有没有都一样；但J-Link不支持ARM10以上的内核。JTAG下载的速度可以达到400~500K，正版价格大约5000人民币（全功能）吧，这么贵基本也不考虑了。

**Multi-ICE原版**

> ARM公司的原创调试工具，支持全系列ARM芯片，现在多少钱我也不知道了，反正在2000~3000人民币这个级别。我这里指的是国内做得比较好的那些，比如Realview之类的。仅仅支持ADS、SDT之类的裸奔代码调试，JTAG下载速度130K左右。虽然这几年Multi-ICE是国内ARM调试绝对的霸主，但现在ARM公司已经停止对ADS的维护了，Multi-ICE会开始走向没落。

**Multi-ICE盗版**

> 国内有很多Multi-ICE的盗版，功能和Multi-ICE原版一样，并口的、USB的都有，价钱几百块人民币，淘宝上到处都有。但是和J-Link盗版相比，不推荐购买。

**J-Link盗版**

> 最近这段时间，J-Link盗版渐渐开始多起来了，淘宝上也很多，功能和原版没有区别。价格大约在几百人民币左右，从性价比来看，推荐购买。我之后还会写一篇用J-Link调试ARM的文章，当你入门之后，绝对无法忍受今天介绍的这个低成本方案的JTAG下载速度，那时就买个J-Link来爽爽。

**U-Link盗版**

> U-Link是Keil公司做的用于ARM和某些增强型8051调试的工具，由于Keil公司做U-Link的时候没有加密，导致现在盗版满天飞，只需要100多块钱就可以买到一个。现在Keil已经被ARM收购，U-Link也是ARM一家的了。U-Link正版在盗版的排挤下，根本没有什么买的必要；U-Link仅仅支持Keil，而且JTAG下载速度仅有20~30K。

**Wiggler电缆**

> Wiggler是世界上最泛滥的一种调试工具，它非常简单，只需要一片74HC244，一个9013，几个电阻就可以。本来Wiggler是Macraigor（[http://www.macraigor.com/](http://www.macraigor.com/)）制作的，可以支持Macraigor的OCDRemote这个GDB Server，可以支持ARM、PPC、ColdFire、MIPS、XScale等多种CPU。后来因为它结构太简单，被人破解后搞得全世界都是，于是Macraigor怒了，现在用OCDRemote必须是Macraigor原厂的Wiggler了……尽管如此，后人又在Wiggler的硬件基础上开发了很多的调试工具，例如H-Jtag；另外也有其他的调试工具增加了对Wiggler的支持，例如OpenOCD。Wiggler电缆的成本特别低，当然它的性能也和成本一样低；用H-Jtag下载速度大约20~30KB/s，用Linux虚拟机下的OpenOCD下载速度大约2KB/s。不过对于囊中羞涩的学生们来说，是一个非常不错的入门工具。

## what is JTAG?

JTAG（Joint Test Action Group）组织定了一个最初是用于测试生产出来的芯片是不是良品的测试接口和标准，在芯片的各个管脚上放上锁存器，然后串起来构成移位寄存器，可以监控芯片管脚的输入和输出；后来大家发现这东西用来搞芯片的在线调试不错，于是就出现了现在JTAG调试风行的局面。再说的明白些，也就是利用JTAG可以控制CPU内核，每个CPU都可以成为自己的“仿真器”，而不需要专用的设备。

“人人都是食神。”——周星星语录。从理论上来说，世界上只需要一种仿真器，哦，确切的说应该叫做JTAG协议转换器，就可以调试所有的兼容JTAG标准的芯片；BDI2000这种超级贵的“仿真器”以及Wiggler这种什么都通吃的便宜货的存在是很合理的事情。

走这条路，应该已经明白了JTAG是什么，所以不用多说了。

## what is GDB?

正像Windows和Linux的对比，集成开发环境比GDB在嵌入式开发领域，拥有更多的用户，但这并不意味的GDB不好。GDB（GNU Project Debugger）是开源软件组织GNU开发和维护的一种调试工具，它能调试目前所有的能跑Linux的CPU，当然ARM也是其中一员。对于初学者来说，不建议使用GDB，还是先从集成开发环境入手，例如ADS、SDT、Keil、IAR之类的。

其实从编译器的层面来讲，集成开发环境和GDB所用的编译器GCC没有什么区别，但集成开发环境里面提供了源文件组织与浏览、工程文件管理、调试等多种功能，用起来很友好。GCC+GDB光学习写相当于工程文件的Makefile就要花很多的时间。但是，一旦你的学习进了一步到了Linux的Loader和内核，集成开发环境就无能为力了。



