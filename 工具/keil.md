# keil mdk 用法学习

## 灵感　

### linux开发主流程(keil项目转linux)
(实验~/myProj/wmt/stm32f10x )
#### linux 下的主要命令工具
1. 一般所所需： make git
2. 特需：
[libopencm3](https://github.com/libopencm3/libopencm3)(Open source ARM Cortex-M microcontroller library)
各种contex M系列固件库，除了ST公司产品外，还支持其它公司产品
build工具：arm-none-eabi/arm-elf toolchain (gcc-arm-embedded)
把libopencm3 git到arm 工程项目中

[freeRTOS](http://www.freertos.org)(实时操作系统)
从网站下载源码，并解压到arm工程项目中

[ARM GNU Embededed Toolchain](https://developer.arm.com)(arm GNU 编译工具)
下载并解压到/opt 目录，将做为常用（公用）工具

**未知option1/2区别，但教程选option2**
- Option1: Home Products Software Development Tools Compilers Arm Compiler Downloads ARM Compiler 6
[Arm Compiler](https://developer.arm.com/products/software-development-tools/compilers/arm-compiler/downloads/version-6)(2018.10.25 6.11版，336Mb)
- Option2: Home Linux and Open Source GNU Toolchain GNU Arm Embedded Toolchain Downloads
[GNU Arm Embedded Toolchain](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)(2018.7 q2 95.9Mb)
以上两个包上传至polo/z_wmt/my_resource/soft/ARM-linux/

[各种docs](https://developer.arm.com/products/software-development-tools/compilers/arm-compiler/documentation)
3. make (Dome中配置先编译libopenm3, 再是其它的目录)

4. 运行st-flash   (自动安装包：stlink-1.5.0-1.fc26.x86_64)

5. 擦/读/写 st-flash [erase read write]

## 知识点滴



## 灵感　(备份)
### keil开发主流程 
(实验taobao卖家源代码 评估板 型号：D-DWM-PG1.7V )
#### IDE安装
1. 安装MDK Keil-v5
　　自动出现窗口"Pack Install" 进到11%不动．
2. STM32F1xx 包
　　有个pack包，双击会被Keil软件识别并unzip
	包名：Keil.STM32F1xx_DFP.2.2.0.pack
	功能：看下这包会装在哪里，再看内部是什么文件
	安装在: C:\Keil_v5\ARM\Pack\Keil\ 为＂STM32F1xx_DFP "
	版本为：2.2.0
	内有很多目录：Boards  Debug  Device  Docs  Flash  RTE_Driver  SVD
3. 破解
	Keygen.exe 要用keil uVision5->File-> License Management中的"Computer ID"

#### 源码编译
1. 打开项目编译
[项目地址](C:\myProj\uwb\MDK-ARM) -> 编译通过

#### 检验原始测距 
1. 上位机查找串口-> 打开串口
2. 扫描设备-> 得到Modbus-id=1 -> 连接设备
3. 稍等一会，自动提示讯取设备成功
4. 改变＂定位模式＂为＂一对一测距" -> 配置载入
5. 开始定位 -> 成功运行

#### 检验自译代码测距
1. 接线 
	评估板	ST-LINK
	GND		GND
	DIO		SWDIO
	CLK		SWCLK
	5V		5V
2. 烧写程序
[STM32 ST-LINK Utility](/home/放在服务器的哪个位置)
3. 读出原bin (MD5=07619df91f25645906fb635a0b22bf8b)(10k)
4. 对比新编译bin(80k)
5. 烧写程序
	打开Utility软件，Target -> Connect
	File -> Open file -> 选择hex文件
	Target -> Program
	Start 开始烧写　 （注：开始地址0x08000000 [为什么呢？](https://blog.csdn.net/qq_33559992/article/details/77676716))
6. 再次测距实验
　　实验ＯＫ


