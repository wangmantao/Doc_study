# MATLAB 学习笔记
## 学习stateflow
[stateflow教程](https://wenku.baidu.com/view/bbdf948f04a1b0717fd5dda0.html?sxts=1544972680576)
[stateflow教程2](http://www.docin.com/p-795966610.html)
[本地demo](C:\Program Files\MATLAB\R2018a\toolbox\stateflow\sfdemos) (结合网上的帮助文档)
本地PDF](C:\users\wangmt\desktop)
[simu例子](open_system('sldemo_fuelsys');
			open_system('sldemo_fuelsys/fuel_rate_control/airflow_calc');)
			
## matlab 命令
open_system('sim模型名字')   打开一个模型demo

## 灵感　
20. 怎么建立一个S函数，并运行
	1. 利用模板，了解其结构，再灵活编和实现目标。
		怎么建立2级S函数--- baidu 最简例子
		(m文件的S函数可以生成代码吗？) 
		[自定义S函数模块构成的Simulink模型C代码转化](https://blog.csdn.net/gjdcgr/article/details/78329148)
		目标1：先用M文件实现了仿真再说

19. 从S函数着手
	(以下时用模型加载s函数, 还可以传递参数给S函数)
	[Use S-Functions in Models](https://ww2.mathworks.cn/help/simulink/sfg/using-s-functions-in-models.html)
	混合C MEX和MATLAB S函数到simulink模型中，要S-Fuction块（在User-defined functions库中），然后在参数对话框中指定s-function的名字。

	(以下是S函数的写法)
	  step 1: 用函数检查某个模型是否存在
	  step 2: 获得某个模型的参数

18. 灵活用代码修改simulink的参数
	[](https://wenku.baidu.com/view/e3b0a406ba1aa8114431d975.html)
17. 我想要的是，要知道simulink --infilted loop 一直whil做的是什么？
	另一个目的是怎么实现串口进行通讯
  if (USART3接收模块状态结束时--已有数据) {
  	USART3接收指针指向BUFFER
  	UsartRcv_B.USART_Receive_o1 = 0;

    /* USART3 pooling receive mode*/
    HAL_UART_Receive(&huart3, G_USART3_RxDataBuffer, ((uint16_T)10U),
                     G_USART3_RxPollTimeOut);
    USART3_RxDataLink.rxStatus = USART_RX_OK;
  }	

16. 流程图可重用，真值表的函数可重用吗？
	[流程图可重用][使用 Pattern Wizard 创建流程图](https://ww2.mathworks.cn/help/stateflow/ug/creating-flow-graphs-with-the-pattern-wizard.html)
	用到时再回头学习
15. 怎么利用matlab 设计系统
	stateflow 有哪些功能，这些功能怎么用到我设计中来？
	对照功能进行画图
	
14. simulink 的实例怎么结合datasheet, 便于更了解MCU的用法
   怎么搜索PDF相关的关键词
	1. 首先快速的打开PDF文件	
	2. 在其中搜索关键词
13. 有个库：　Rapidstm32
12. 回到windows+matlab
	matlab启动也不快
	环境都装置全了，在windows下了解STM32仿真的过程
	代码可以生成了，我直接的目的是想要控制一个Gpio
	实验方法是：
		从原示示例中复制一份GPIO项目，找出生成的代码部分
			有Driers / EWARM(ewp文件定义了库文件) / Inc / SimpleGpio /SimpleGPIO_stm32 / Src / slprj / --> for IAR 生成
			(https://blog.csdn.net/dongganxiao_maidou/article/details/64904944?locationNum=14&fps=1) --> for keil 
			-------------> (ProjectManager.TargetToolchain ioc文件定义的编译ide)

		改变新项目的cpuConfig(创新指定ioc文件), 然后重新生成代码，并在 MDK中打开
		编译并烧入F103中查看动作

11. matlab 官方的　stm hardware
	1. 为Discovery Board 安装支持包
		支持硬件和它的特性
		模块库有相应模型
		有使用Discovery Bord的例子
	2. 为Discovery Board 安装驱动

10. 砍去多余的库与工具
	* /opt/MATLAB (插件)
	3p.insert/gnuarm-armcortex.insert ---> 是arm toolchain, 将不要用它，要用/opt/arm-gcc/(高版toolchain)

9. 关于Exsamples 移到了/home/wmt/temp/test/中

6. matlab在用户模式下配置插件和toolchain
	* /root/.matlab/matlab.settings
		41行设置Add-Ons      (/home/wmt/.matlab/下设置好了，把root下的Document/MATLAB删除-->因为它转移到了opt下)
	* /root/.matlab/slhistory.settings
		6行设置toolchain
5. wmt use root pacage:
	* 硬件实现
	硬件板：    ARM Cortex-M3(QEMU)
	硬件实现：　ert.tlc       ---> 指向path? (Embeded Corder : /usr/local/MATLAB/R2018a/rtw/c/ert.tlc)
	设备供应商：ARM 兼容 type: cortex
	* 代码生成
	系统目标文件: ert.tlc
	Toolchain: GNU Tools for ARM Embeded Processors ----> 在什么文件中定义?又被定义在哪？
	......

4. STM32-MAT LINUX下不能用
	**附加功能管理器** 84
	Embedded Coder Support Package for ARM Cortex-M Processors 版本 18.1.1 
	第三方软件:
	GNU Tools for ARM Embedded Processors
	CMSIS
	**附加功能管理器** ??

	这是以下项所需要的:
	Embedded Coder Support Package for ARM **Cortex-M** Processors

	其实还有个： (for vision)
	Arm **Cortex-A** Embedded Coder Support Package


1. 嵌入式coder started
	s1: 为生成代码而配置模型
	用的不是setm32成功了．
2. 我要实现smt32f1x的matlab仿真, 然后再生成代码，然后再烧入iC，然后再用PIL仿真

3. 错误
	Error evaluating 'MaskDialog' callback of GPIO_Write block (mask) 'untitled/GPIO_Write'. Callback string is 'GPIO_Write_callback('Port_Select');' 
	未定义函数或变量 'Pin_idx'。
	No GPIO configured as output!
	
##知识点滴：
3. QEMU emulator 开源的虚拟机（此处虚拟ARM M3运行simulink的模型）
2. 关于matlab中，Target System工具包是由STM32-MAT/TARGET提供
	设定了pathtool才会有
1. 安装的问题和软件源 (在ARM笔记中)

## 资源
[Real-time workshop RTW](https://baike.baidu.com/item/Real-time%20workshop/9001254?fr=aladdin)
