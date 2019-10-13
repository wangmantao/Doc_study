# FreeRTOS & ARM 用法 (主要Linux下的STM32开发方法)
    现在F407VE项目用的是F4 1.23的库（其它库可删掉）
## 资源
1. [stm社区](http://www.stmcu.org.cn/)(下载快)
2. st官方注册: wangmantao@163.com st456321 you mother's name? hmh
3. **Matalb 相关部分转移 st matlab**
-	 [matlab 配置](https://alpha-ss.sohu.com/infonews/article/6345903537284710401) 
	matlab重下载（安装）
-	[安装教程和资源](https://www.macxin.com/archives/4722.html)
	This error occurs when your computer cannot load a certain font display library through MATLAB. 
	mv /usr/local/MATLAB/R2018b/bin/glnxa64/libfreetype* exclude/
-	[Consolas YaHei hybrid.ttf](https://www.cnblogs.com/Qling/p/9487647.html)matlab字体
-	[Matlab&Simulink开发STM32F4](https://blog.csdn.net/sky_in_my_mind/article/details/51194635)
4. [keil device库](http://www.keil.com/dd2/stmicroelectronics/stm32f407zg/)
5. [stm32cube 库例程](https://wenku.baidu.com/view/9ff93ca303d276a20029bd64783e0912a2167cbd.html)
6. [用C 语言实现面向对象编-潘秀才](https://wenku.baidu.com/view/4c94420590c69ec3d5bb752f.html)
7. [轻量级面向对象C语言编程框架lw-oopec源代码的使用方法](https://wenku.baidu.com/view/2d820d6bcfc789eb172dc8c1.html?re=view&rec_flag=default&sxts=1548278017935)
8. [FreeRTOS队列](https://blog.csdn.net/nippon1218/article/details/79001806)
9. [cmsis-rtos]()
10. [STM32 USB CDC 虚拟多串口 ](http://www.stmcu.org.cn/module/forum/thread-613510-1-1.html)
11. [stm32f4xx 固件下载地址] (https://my.st.com/content/my_st_com/zh/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-configurators-and-code-generators/stm32cubemx.license=1550590120810.product=STM32CubeMX.version=5.0.1.html)

## 灵感
107. stm32 完成ADC
    https://blog.csdn.net/yhl_sophia/article/details/83744666

        查询方式

        uint32_t value = 0;
        float vol = 0;
        while(1)
        {
           /*USER CODE BEGIN WHILE*/
           HAL_ADC_Start(&hadc);
           HAL_ADC_PollForConversion(&hadc,HAL_DELAY_MAX);
           value = HAL_ADC_GetValue(&hadc);
           vol = (float)(value *3.3/4096);
           printf("%.2f",vol);
           HAL_Delay(1000);
           /*USER CODE END WHILE*/
        }


106. 重新设置的freertos.c S_buf start:ok
    当pc系统准备好后，不论PC系统再次关掉，还是再开，ＭＣＵ都会对应每次从pc来的start:ok信号，让测试重新开始　

105.  arm加密
    segger里打开flash安全选项
    置位的加密
    芯片id号: http://www.51hei.com/bbs/dpj-42683-1.html  

         if(FLASH_GetReadOutProtectionStatus() != SET)
        {
        FLASH_Unlock();
        FLASH_ReadOutProtection(ENABLE);
        FLASH_Lock();
        }
         ----------------
         void Check_Flash(void){
             FlagStatus status = RESET; 
            status = FLASH_GetReadOutProtectionStatus(); 
             if(status != SET) { 
                FLASH_Unlock(); /* Flash 解锁 */ 
                /* ENABLE the ReadOut Protection */ 
                FLASH_ReadOutProtection(ENABLE); //读保护使能 
                FLASH_EnableWriteProtection(FLASH_WRProt_AllPages); //写保护使能 
                Reset_System(); }} 
104.  uint8_t UserTxBuffer[] = reToPc; 初始化字串不行

103. strtok 嵌套替代品　strtok_r
    示例：　/home/wmt/temp/test/c_test

102. scpi
MnemonicName488.2 Section
*CLS Clear Status Command10.3
*ESE Standard Event Status Enable Command10.10
*ESE? Standard Event Status Enable Query10.11
*ESR? Standard Event Status Register Query10.12
*IDN? Identification Query10.14
*OPC Operation Complete Command10.18
*OPC? Operation Complete Query10.19
*RST Reset Command10.32
*SRE Service Request Enable Command10.34
*SRE? Service Request Enable Query10.35
*STB? Read Status Byte Query10.36
*TST? Self-Test Query10.38
*WAI Wait-to-Continue Command10.39
101. 调试遇到的问题
   1. 没有.m2　资源库
   2. 串口　librxtxSerial.so 库没有装 (/usr/java/jre1.8.0_151/lib/amd64/librxtxSerial.so /usr/java/jdk1.8.0_201-amd64/lib/amd64/)
   3. 串口　jar 依赖 ()
   4. /run/local 所属和权限给本地用户
    ----其它注意问题------
   1. /ect/udev　下的stm32.rules 
   2. cp210.ko 加上万用表的id, 重新编译放入modules并加入新的module开机启动

100. CDC发送与接收
	uint8_t  USBD_CDC_RegisterInterface  (USBD_HandleTypeDef   *pdev, 
	uint8_t  USBD_CDC_ReceivePacket      (USBD_HandleTypeDef *pdev);
	./Middlewares/ST/STM32_USB_Device_Library/Class/CDC/Inc/usbd_cdc.h -------------------
 
	uint8_t  USBD_CDC_TransmitPacket     (USBD_HandleTypeDef *pdev);
	./Middlewares/ST/STM32_USB_Device_Library/Class/CDC/Inc/usbd_cdc.h -------------------

	uint8_t CDC_Transmit_FS(uint8_t* Buf, uint16_t Len); 
	./Inc/usbd_cdc_if.h -------------------
	(为什么没有CDC_Receive_FS   --- 因为它在Ｃ文件中定义的) --怎么才能用好它呢?
[用法示例](https://www.cnblogs.com/yuweifeng/p/5843688.html)	

	三、数据接收 [](https://blog.csdn.net/amonyoung/article/details/46124829)
	
	首先初始化,通过USBD_CDC_SetRxBuffer(USBD_HandleTypeDef *pdev, uint8_t *pbuff)给USB库指接收缓冲区，让USB控制器填收的数据。

	响应CDC_Itf_Receive(uint8_t* Buf, uint32_t *Len)回调函数接收数据. USB控制器收到数据即回调它,输入参数指定的数据缓冲区。
	CDC_Itf_Receive()函数在接收标记完这些数据之前别急着返回，不然你可能就摸不到剩下的数据了。
	USBD_CDC_ReceivePacket()函数的作用是复位OUT端点接收缓冲区，
	CDC_Itf_Receive()函数在接收完数据之后要调用该函数复位缓冲区。

	有一点需要注意，CDC_Itf_Receive()函数是USB中断回调函数，因此应该尽快返回，只能在函数内对数据做简单的标记或处理。如果要对数据进行繁重的处理，应该在main()函数或工作进程内处理。

	3、PC-->USB接收数据
这个过程略微复杂，需要从回调函数CDC_Itf_Receive()获取PC发送来的数据，并在合适的地方处理数据。我的例子中是在main()函数中处理。CDC_Itf_Receive()函数在usbd_cdc_interface.c文件中。

	------------------
	usb_device.c里面仅包含一个USB设备函数初始化函数 MX_USB_DEVICE_Init(),在程序开始时调用。
	usbd_cdc_if.c为USB的CDC类应用层文件，里面包含虚拟串口的接收，发送和控制等函数。
	usb_desc.c包含USB的描述符，以及USB枚举处理等函数。
	usb_conf.com为USB管脚配置文件，包含引USB引脚初始化以及参数设置，中断回调函数等。

	--------
	CDC_ReceivePacket()与　return (USB_OK)之间，添加数据处理函数
	for(i=0; i< *Len; i++)
		app_rx_buf[i] = Buf[i];
	rx_f= true; // 数据接收完毕，让处理果的程序知道

99. 8341万用表的命令组成
 	CONFigure:VOLTage:DC
	

98. udev 写stm32
	http://bbs.elecfans.com/jishu_1704452_1_1.html
	https://www.cnblogs.com/fah936861121/p/6496608.html (规则详细写法)
	https://bbs.archlinux.org/viewtopic.php?id=207885 (modprob cdc_acm ; echo v p ->new_id)
97. [Can not open Putty with USB to serial adapter] (https://www.centos.org/forums/viewtopic.php?t=21271)
	grep 2184 /lib/modules/*/modules.alias | grep 0030 
	0557(vid) 2008(pid)
	找到..
	 /lib/modules/4.11.8-300.fc26.x86_64/modules.alias:alias usb:v2184p0036d*dc*dsc*dp*ic*isc*ip*in* cdc_acm 
	/lib/modules/4.13.12-200.fc26.x86_64/modules.alias:alias usb:v2184p0030d*dc*dsc*dp*ic*isc*ip*in* cdc_acm
	...
	cdc_acm 本系统有相关驱动，但没有.ko扩展名

	#dmesge |grep 3-1
	659 [    1.821556] usb 3-1: New USB device found, idVendor=2184, idProduct=0030 
	660 [    1.821561] usb 3-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3 
	661 [    1.821563] usb 3-1: Product: GDM834X VCP PORT 
	662 [    1.821566] usb 3-1: Manufacturer: Silicon Labs 
	663 [    1.821568] usb 3-1: SerialNumber: GES885640

	
96. 安装gwinstek linux 串口
	ptython 不成功（github中的项目）
95. [Debug Message To OpenOcd](https://electronics.stackexchange.com/questions/149387/how-do-i-print-debug-messages-to-gdb-console-with-stm32-discovery-board-using-gd)
	发信息到Opencd片段
	const char s[] = "---执行 set\n";
	uint32_t m[] = { 2/*stderr*/, (uint32_t)s, sizeof(s)/sizeof(char) - 1 };
	send_command(0x05/* some interrupt ID */, m);


94. 关于数组指针
	数组指针指array * (*p)的是，一个数组的地址
        p -  就是数组（放了很多指针的数组）的的地址 
	*p - 得到这个地址内部所放的指针，即指向数组第一个元素的地址
	**p - 再把第一元素的地址的内容（即第一个元素的实例）

93. osStatus osThreadYield();//切换到另外一个准备运行的线程

92. [gdb optimized out](https://blog.csdn.net/zhangxiao93/article/details/51934354) -o0 编译－－不采用优化

91.  HardFault_Handler
	HardFault_Handler () at Src/stm32f4xx_it.c:102
	 <signal handler called>
	 0x08003d1e in xTaskIncrementTick ()
	    at Middlewares/Third_Party/FreeRTOS/Source/tasks.c:2615
	0x08004cca in SysTick_Handler ()

90. [osEvent]()
	(gdb) p sta
	$1 = {	status = osEventMessage, 
		value = {v = 234, p = 0xea, signals = 234}, 
	  	def = {mail_id = 0x20000a90 <ucHeap+1860>, 
	    	message_id = 0x20000a90 <ucHeap+1860>}}

89. [osStatus](http://www.keil.com/pack/doc/CMSIS/RTOS/html/cmsis__os_8h.html#ae2e091fefc4c767117727bd5aba4d99e)

85. LED toggle 片段
	if(HAL_GPIO_ReadPin(GPIOA, LED3_Pin) == GPIO_PIN_RESET)
       		HAL_GPIO_WritePin(GPIOA, LED3_Pin, GPIO_PIN_SET);
         else
                HAL_GPIO_WritePin(GPIOA, LED3_Pin, GPIO_PIN_RESET);

84. CDC 发送命令 片段
	uint8_t UserTxBuffer[] = "cdc for test ok? ..\r\n";
        CDC_Transmit_FS(&UserTxBuffer, sizeof(UserTxBuffer));

83. cmiss-rtos
真的是很简单，直接调用vTaskSuspend用于挂起某个任务，调用vTaskResume用于继续某个任务

82. 
	可能是初始化配置的问题,定时器的初始化，晶振等
	RCC_ClkInitTypeDef RCC_ClkInitStruct;   //结构体初始化
	RCC_OscInitTypeDef RCC_OscInitStruct; //结构体初始化
	__HAL_RCC_PWR_CLK_ENABLE();  //使能电源控制时钟   
	__HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE1);//设置调压器输出电压级别1
	这个设置用来设置调压器输出电压级别，以便在器件未以最大频率工作时使性能与功耗实现平衡
	RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI; //时钟源为HSI内部高速时钟
								       //RCC_OSCILLATORTYPE_HSE   高速外部时钟
	RCC_OscInitStruct.HSEState = RCC_HSE_OFF;  //关闭HES
	RCC_OscInitStruct.HSIState = RCC_HSI_ON;    //打开HSI
	RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON; //打开PLL
	RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSI;//设置PLL时钟源为HSI
	RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL4;//PLL VCO输入时钟的倍频系数
	RCC_OscInitStruct.PLL.PLLDIV = RCC_PLL_DIV2;//PLL VCO输入时钟的分频系数
	RCC_OscInitStruct.HSICalibrationValue = 0x10;//HSI校准调整值
	HAL_RCC_OscConfig(&RCC_OscInitStruct);  //初始化
	RCC_ClkInitStruct.ClockType = (RCC_CLOCKTYPE_SYSCLK | RCC_CLOCKTYPE_HCLK | RCC_CLOCKTYPE_PCLK1 | RCC_CLOCKTYPE_PCLK2);    //选中PLL作为系统时钟源并且配置HCLK,PCLK1和PCLK2
	RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK; //设置系统时钟源
	RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1; //AHB分频系数为1
	RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;  //APB1分频系数为1
	RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;  //APB2分频系数为1
	HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_1);//初始化并同时设置FLASH 的延迟周期为1

81.  文件交互引用的情况
	usbd_conf.c 引用了ubd_def.h -> 它又引用了usbd_conf.h
 
80.	Src/main.c:52:1: error: unknown type name 'USBD_HandleTypeDef'
	 USBD_HandleTypeDef;
       ./Core/Inc/usbd_def.h  
		usbd_def.h 是什么文件？定义了usb设备的４个状态 [usb学习笔记](http://www.eeworld.com.cn/mcu/2018/ic-news091441328.html)
		cubeMX在有没引用头文件，以哪引用 
		demo项目文件有没有用到usbd_def.h(.c）文件 ---两个项目都在同在usbd_desc.h中引用了def
		怎么才能排解这个错误? --- 用demo的main.h
		---- 新错:
	 		usbd_cdc_interface.h: No such file or directory--> 从demo项目复制
		---- 新错:
			stm32446e_eval.h: No such file or directory(作用－－定义评估板的硬件资源）-->remove include from main.h
			
		Src/main.c:73:28: error: 'VCP_Desc' undeclared --> 从demo项目复制usbd_desc.h(c)覆盖之

		Src/main.c:76:36: error: 'USBD_CDC_CLASS' 　--> 同上
		Src/main.c:79:45: error: 'USBD_CDC_fops'    --> 同上
		Src/main.c:149:23: error: 'RCC_PeriphCLKInitTypeDef {aka struct <anonymous>}' has no member named 'PLLSAI';
		Src/main.c:151:40: error: 'RCC_PLLSAIP_DIV8' 


		Drivers/STM32F4xx_HAL_Driver/Inc/Legacy/stm32_hal_legacy.h:2883:42: error: 'RCC_PERIPHCLK_CLK48' undeclared 
		Src/main.c:152:46: note: in expansion of macro 'RCC_PERIPHCLK_CK48'
		   PeriphClkInitStruct.PeriphClockSelection = RCC_PERIPHCLK_CK48;


		Src/main.c:153:23: error: 'RCC_PeriphCLKInitTypeDef {aka struct <anonymous>}' has no member named 'Clk48ClockSelection'

70. 怎么才能在linux下，用makefile编译
	实验一，
		回件库demo的地址(cdc的解决)
		/home/wmt/STM32Cube/Repository/STM32Cube_FW_F4_V1.23.0/Projects/STM32446E_EVAL/Applications/USB_Device/CDC_Standalone
		把相关文件复制到mx项目中
	实验二, 利用f407ve项目的make 文件
		把demo中的main.c 放入f407ve项目中
		make 看是否报错缺少的文件
		逐步解决所有的错误	

60. Windows mdk编译后， Linix miniCom通讯
	OK.
59. 在windows　MDK已经完成demo项目的编译　()
	MDK USB_DEVICE 项文件结构
	A_1. Middlewares/STM32_USBD_Library/Core
		usbd_core.c				--> 定义USBD_Init();
		usbd_ctlreq.c
		usbd_ioreq.c

	A_2. Middlewares/STM32_USBD_Library/Class/CDC
		usbd_cdc.c                             --> (h文件)USBD_CDC
	----------------------------------
	B_1. Application/MDK-ARM
		startup_stm32f446xx.s

	B_2. Application/User  		-> mx项目对比
		main.c
		stm32f4xx_it.c		ok
		stm32f4xx_hal_msp.c	ok
		usbd_desc.c		同		    --> 定义了USBD_DSCRIPTOR	
		usbd_cdc_interface.c	no	      (少)  --> 定义了USBD_Device ; USBD_CDC_ItfTypeDef  USBD_CDC_fops
		usbd_conf.c		同	
					usb_device.c  (多)
					usbd_cdc_if.c (多)	
	=============================
	usbd_core.h 定义的成员函数：
	USBD_StatusTypeDef USBD_Init(USBD_HandleTypeDef *pdev, USBD_DescriptorsTypeDef *pdesc, uint8_t id);
	USBD_StatusTypeDef USBD_RegisterClass(USBD_HandleTypeDef *pdev, USBD_ClassTypeDef *pclass);
	USBD_StatusTypeDef USBD_Start  (USBD_HandleTypeDef *pdev); 

	usbd_cdc.h 定义的成员函数：
	uint8_t  USBD_CDC_RegisterInterface  (USBD_HandleTypeDef   *pdev, USBD_CDC_ItfTypeDef *fops);

	

58. HAL库延时 (优选方式)获取系统时钟计时，非阻塞式延时 片段
	 void delay_ms(int32_t nms) 
	 {
	  int32_t temp; 
	  SysTick->LOAD = 8000*nms; 
	  SysTick->VAL=0X00;//清空计数器 
	  SysTick->CTRL=0X01;//使能，减到零是无动作，采用外部时钟源 
	  do 
	  { 
	       temp=SysTick->CTRL;//读取当前倒计数值 
	  }
	     while((temp&0x01)&&(!(temp&(1<<16))));//等待时间到达 
	     
	     SysTick->CTRL=0x00; //关闭计数器 
	     SysTick->VAL =0X00; //清空计数器 
	 }

57. dd if=/dev/zero of=/dev/ttyACM0 bs=8k count=1000 
	usb ACM0　压力测试
56. 在default中定义任务,并让任务在wait状态

55. cmsis-rtos 信号：semaphore
	osSemphoreWait() 
54. 因为要用stm32CubeMx, 不得已要用cmsis-rtos,　重新学习它的API.

53. Static allocation / Dynamic allocation 区别
52. xQueueSend 传递指针数组(地址)解决方案
	1. 建立一个指针函数，返回指针数组的变理名（实为指向数组指针的地址）
	2. 用一个void \*指针接收这个地址(此处void 类型并没有生产一个新的地址，它的地址就是上个指针函数返回的地址)
	3. 用queueSend这个地址（要用＆取出指针所在的地址）
	4. 在queueReceive接收部分先定义一个void 指针变量名
	5. 在Receive的参数把接收的数据写入void指针的地址（&void指针变理名）
	6. 把刚定义的void型指针赋给新定义的数组指针(Ry* (*rys)[1])就可以了。
51. 指针数组& 数组指针　（用地址实现实例再操作）解决方案
	Rys变量值是：70000af0   		printf("Rys变量值是：%x\n", Rys[0]);
	Rys变量值是：70000b50   		printf("Rys变量值是：%x\n", Rys[1]);
	Rys变量值是：70000bb0   		printf("Rys变量值是：%x\n", Rys[2]);
	Rys索引0值是：77a9be60   		// 存入实例地址的索引地址 (Rys数组的地址)
	Rys索引1值是：77a9be68   		printf("Rys索引0值是：%x\n", &Rys[0]);
	Rys索引2值是：77a9be70   		printf("Rys索引1值是：%x\n", &Rys[1]);
	第一个元素的索引地址是：77a9be60   	printf("Rys索引2值是：%x\n", &Rys[2]);
	第二个元素的索引地址是：77a9be68
	第二个元素的索引地址是：77a9be70   	// 做一个数组指针，指向Rys数组
	第一个元素的地址是：70000af0   		Ry* (*rys)[1] = Rys;
	第二个元素的地址是：70000b50   		printf("第一个元素的索引地址是：%x\n", rys);
	第二个元素的地址是：70000bb0   		printf("第二个元素的索引地址是：%x\n", rys+1);
	被发送的对象实验：ry1的值是：obj_ry1   	printf("第二个元素的索引地址是：%x\n", rys+2);
	被发送的对象实验：ry2的值是：obj_ry2   
	被发送的对象实验：ry3的值是：obj_ry3   // 解析指针所指向的实例地址
						printf("第一个元素的地址是：%x\n", **rys);
						printf("第二个元素的地址是：%x\n", **(rys+1));
						printf("第二个元素的地址是：%x\n", **(rys+2));
	   
						// 把实地址给调出来实例化
						Ry* r1= **rys;
						Relay* re1 = SUPER_PTR(r1, Relay);
						printf("被发送的对象实验：ry1的值是：%s\n", re1->name);
						Ry* r2= **(rys+1);
						Relay* re2 = SUPER_PTR(r2, Relay);
						printf("被发送的对象实验：ry2的值是：%s\n", re2->name);
						Ry* r3= **(rys+2);
						Relay* re3 = SUPER_PTR(r3, Relay);
						printf("被发送的对象实验：ry3的值是：%s\n", re3->name);


50. 数组指针偏移量
	arr[4] arr的地址是0x00
	arr首元素的地址是：&arr[0]
 	&什么+1 就是&符号后面的内容(占据的地址单元数)+(指针起始位置)
	arr[0]占据4地址单元
	所以arr +1 == &arr[0]+1 = 4 + 0x00 = 0x04; 而 
	&arr+1 = 0x10; 这里&后面(arr是个数组名)就可以当成是arr这整个数组占据的地址单元数+地址.它占据16个地址单元 &arr+ 1 = 16 + 0x00 = 0x10.
	一维数组中各元素也是如此,arr[0]+1等于 arr[1]同样适用,arr[0]是int 类型在内存中占用四个字节,所以+1也是跳到第5个字节就是arr[1]的字节那儿,所以就是arr[1]了

49. queue 发送一个指针
	声明一个array（非地址变量）
	再声明一个指针（指针类型为以上变量类型）
	发送这个指针（要对指针取地址）
	－－－－
	接收任务：
	定义一个类型的指针变量
	xQueue 接收指针（要对针针取地址）

48.  lw-oopc 扩展一个类extends
	EXTENDS(Animal);
	- 调用继承的方法稍显麻烦：
	((Animal*)bird)->Eat((Animal*)bird);
 	- 当EXTENDS语句在定义类时并不在第一句（下一篇将揭露各个宏的真实面目）时，最稳妥也显得繁琐一点的方式为：
	SUPER_PTR(bird, Animal)->Eat(SUPER_PTR(bird, Animal));

48. 怎么写一个实例的集合：
	仿照fish Alimaos (lw-oopc demo)
	 Animal* animals[2] = { 0 };   说明：数组类型指针　数组名和[容量]　= {初始化０－什么也没有}
	 animals[0]= ry1;                    把指针放入数组

47. 传递对象的指针，然后再通过指针把对象重构出来
	Ry* ry1 = Ry_new();
	Ry* pRy1 = ry1;   -> 就得到了同样的对象

46. freeRtos 任务的挂起和恢复
'''
	TaskHandle_t pump_task_handle  = NULL;  	    // 要有个任务句柄
	if(eTaskGetState( pump_task_handle ) != eRunning)    //启动函数
    	   vTaskResume( pump_task_handle );		  	//恢复
	if(eTaskGetState( pump_task_handle ) != eSuspended) //暂停函数:
	   vTaskSuspend( pump_task_handle );              	// 挂起 
'''
45. [FreeRTOS学习记录1-熟悉FreeRTOS的命名规则](https://blog.csdn.net/liukais/article/details/78958850)
44. [消息队列知识](https://www.cnblogs.com/yangguang-it/p/7198622.html)
	消息队列句柄建立后，各个相关任务建立时都用这个队列句柄

43. 为了不在MCU上浪费调试时间，在linux上开发freeRTOS.
[原例](https://www.freertos.org/FreeRTOS-simulator-for-Linux.html)
[例](https://github.com/crazyskady/FreeRTOS_study)
[在Linux下实现FreeRTOS的简单模拟器](https://blog.csdn.net/crazyskady/article/details/79405813)
   本地：　/home/wmt/opt/FreeRTOSv10.1.1/FreeRTOS/test/FreeRTOS_study/Simulator_Linux

42. 在main函数中，启用trcRecorder中统一的API即可！启动追踪函数为 vTraceEnable(TRC_INIT)
41. 任务被删除 vTaskDelete() 
40. 利用阻塞态实现延迟 void vTaskDelay( portTickType xTicksToDelay );
39. 让一个任务进入挂起状态 vTaskSuspend() 恢复 vTaskResume() 或TaskResumeFromISR() 
38. tick中断频率 configTICK_RATE_HZ 
37. 改变任务优先级 vTaskPrioritySet()

36. 开发流程
	先建立USB通讯，再建立commu对象接收数据，再用flash分块存储相应的数据，再用A_process调用相应的数据，生成RY动作序列.
	- 定义数组变量——对象名字
	- 定义数组变量——对象io名称
	- 定义数组变量——对象io初值
	- 定义数组变量——对象延长时间 (延长时间--在状态set才使用,reSet返回不用。有两种情况：１，某ＲＹset后开始计时，时完后reset；２，某ＲＹset后，没有计时，等待某信号才reset)

35. 关于气缸与继电器联动的的方案
	每个RY动作都按一定流程，有没有不接流程的。一个ＲＹ动作还未结束，需要另一个ＲＹ动作，即：RY0-之后不同时间来控制RY1,RY2
	所以程序做成可以定义哪个RY先动作，哪个后动作。A_process 根据查表来控制相应的继电器(把继电器对象，按一定的名称顺序初始化到一个数组中，数组中的对象逐个动作)

	有的ry会根据结果来动作，比如OK, 是由station的结果来动作的，station (sendData / receiveDate / jugement　/ 参数的设定管理)

	数据相关，还要有一个可以发送和接收串口数据的对象：
		(USB是用的什么端口----STM32-USB Slave 接口 PA11/USB_DM PA12/USB_DP)
[ST系列单片机+CubeMX+USB虚拟串口](https://blog.csdn.net/qq_16481211/article/details/81386579)
		该对象要保证有两个对象同时请求数据发送或接收时，可以让任排队，因为只有一个USB串口。
		关键词是：　1.HAL UART 怎么通讯, 2.　串口怎么多任务通迅·

34.  用oopc实现点灯实验
	打开IOＣ项目，添加LED配置，然后在一个任务中，控制这个led
	PA6/PA7(LED2/LED3) 低电压点亮 R13/R14(510ＯＭ)　３.3/ 510 = 6.5mA
	K0/K1 (PE4/PE3) 连接端口到地

33. \#\# 双井号的意义
	双井号两边的参数相连接形成一个新的符号，可以动态创建新的函数(代码生成)

33. GPIO端口的HAL库操作
	HAL_GPIO_WritePin(LED1_GPIO,LED1_GPIO_PIN,GPIO_PIN_SET)    //置高
	HAL_GPIO_WritePin(LED1_GPIO,LED1_GPIO_PIN,GPIO_PIN_RESET) // 置低
	怎么在c中用宏定义出“LED_GPIO_PIN” 是个变量替换形式
	有定义宏的方式：先把LED1_GPIO, LED1_GPIO_PIN 中的“LED1”作为宏的参数，整个一条命令“HAL_GPIO_WritePin”作为宏来执行。
	所以定义两个宏，一个用来置高，一个用来拉低

32. lw-oopc 要点
	[头文件]　声明抽象类 -> 声明类　->　[Ｃ文件] ABS实函数 -> ABS_CTOR(抽象类构造) -> 类实函数 -> CTOR(类构造_subpe_ctr)　->  [主程序]　new 类实例　-> SUPER_PTR(类，抽象类) -> 实例成员可以调用 
	每个实例的函数要调用，调用的是哪一个，首先在CTOR中进行FUCTION_SETTING(),
	实便可以调用抽象函数的方法，先要把实例的指针转为抽象的指针。 SUPER_PTR(实例类，超类) ----　让实例具有超类的成员 
	经验总结：
	　类中已经申明的成员函数，在类实例化（class_new())后可以直接调用成员函数。
	　但想用基类的成员（类中并申明），首先用SUPER_PTR把类转换成基类的指针，再进行操作。

31. 学习面向对象的C项目
	- 新建一个项目目录在temp文件夹中
	- 借用lw-oopc.h头文件，并编写main.c　引用这个头文件
	- 打开百度文库的秀教授示例，先看懂，再抄写试运行
	- 在以下灵感30项目的其础上完成面向对象的设计

30. 建立一个rtos mx项目[例](http://www.stmcu.org.cn/module/forum/forum.php?mod=viewthread&tid=615921)
	- 配置GPIO模式:
	-  配置HCLK 72m (HSI 16M) HCLK ：AHB总线时钟，由系统时钟SYSCLK 分频得到，一般不分频，等于系统时钟，HCLK是高速外设时钟，是给外部设备的，比如内存，flash。
	- 勾选FreeRTOS
	- 具体配置 tasks＆queues	
	- EDIT TASK (见图) 任务名　函数　优先级(三个不同的级别)
	- 不建议时基源为systick -> SYS Timebase source = tim1
	- project settings 保存项目 

	-------------
	- 系统生成了3个启动任务的函数startXXtask
	- Create th thread(s)
	- while 在osKernelStart() 下面，故里面不写程序
	- 任务函数 for　里有个 osDelay(1) -> 让调度器有机会调其它任务
	-	for 里写任务

29. gdb操作
	arm-none-eabi-gdb fileName.elf  -> accepting 'gdb' connection on tcp/3333 （自动连上了）     ---> 第二个窗口再运行
	
28. 用命令操作OpenOCD
	openocd -f interface/jlink.cfg -f target/stm32f4x.cfg                          ---> 第一个窗口先运行
	板子　<-> OpenOCD <-> gdb
	用到两个配置文件：接口（jlink) 和　目标板（）
	telnet localhost 4444 ---> 就可以控制OpenOcd相连的target
   可以自定义cfg文件，夹杂常用的命令[openocd.cfg](https://www.cnblogs.com/sannyas/p/3792748.html)
	
27. 用命令操作JLINK
 - 连接Targ 并运行脚本
	JLink.exe -device STM32F407VE -if JTAG -speed 4000 -jtagconf -1,-1 -autoconnect 1 -CommanderScript C:\Work\JLinkCommandFile.jlink
	JLinkExe -device STM32F407VE -if JTAG -speed 4000 -jtagconf -1,-1 -autoconnect 1 -CommanderScript /home/wmt/myProj/XiaDengMing/arm-charger/cmdList.jlink
 - 烧写程序
    loadfile  c:/tmp/STM32F4-FreeRTOS/binary/FreeRTOS.bin
- 把烧写程序简单化，可以随便打开一个终端就可以烧写程序 (bin/flashCharge)
   JLinkExe -device STM32F407VE -if JTAG -speed 4000 -jtagconf -1,-1 -autoconnect 1 -CommanderScript /home/wmt/myProj/XiaDengMing/arm-charger/cmdList.jlink

26. FreeRTOS支持5种动态内存管理方案，分别通过文件heap_1，heap_2，heap_3，heap_4和heap_5实现.
25. 任务创建、信号量、消息队列、事件标志组、互斥信号量、软件定时器组等需要的RAM空间都是通过动态内存管理 , FreeRTOSConfig.h文件定义的heap空间中申请的
24. cubeMx 用FreeRTOS, 利用RTOS的多任务
	[CubeMX中FreeRTOS配置参数及理解](https://www.cnblogs.com/greenlight-xj/p/9683320.html)
	把项目配置成RTOS项目
	建立三个任务，每个任务侦测自己的状态信号
	每个任务处理自己的事件及逻辑
	
23. cube HAL库, 根据变量值控制LED0,LED1,LED2的亮的状态 
	 变量值通过某个地方存储空间（程序表），用程序表结构体

22. 外部模式来设计STM32模型
	外设： ADC IN1
	软件实现： 从simulink 中读取 ADC1 IN1的值 
	CubeMax配置IOC ： P87
	创建slx模型，并导入STM32config块
	ADC_Read 并转换电压的形式 -》 到SCOPE输出查看
	在simulink中转到external模式
	build & keill compile 
	查看ST-LINK的串口号，待用

21. 固纬 GDM USB CVP（或CDC）想用STM32——USB 连接该虚拟串口
	[mcu-USB当虚拟串口](http://www.stm32cube.com/question/160)
	stm32 f407作为USB HOST，外接虚拟串口设备，请问有人做过么？
		[git项目](https://github.com/mori-br/STM32F4HUB)
		[项目所用ac6](http://www.stmcu.org.cn/module/forum/thread-611124-1-1.html)
20. __weak 是什么类型？
19. @brief @retval 是什么，作用是什么？
18. simulink代码生成位置
17. 定下目标：仿真gpio, 实现gpio
	已经实现。生成的代码要注视掉一部分;还有main部分不能循环的
18. CubeMx 下载的repostory位置和库的安装位置, 以便将来删掉节省空间。
	C:\Users\wangmt\STM32Cube\Repository


16. matlab的嵌入式的用法demo 
	arm_cortex_m_gettingstarted 编译通过有diagnostics日志

15. Matlab 嵌入式ARM支持包放在/root/Document
	file:///root/Documents/MATLAB/SupportPackages/R2018a/help/supportpkg/armcortexm/index.html

14. 多重arm-gcc 工具链
	(两个工具链差不多，待熟悉后删除一个，留下哪个待定)
	1. /opt/gcc-arm/bin/arm-none-eabi-gcc (最新，由gdb.pdf为证)
	2. /root/Documents/MATLAB/SupportPackages/R2018a/3P.instrset/gnuarm-armcortex.instrset/gcc-arm-none-eabi-5_2-2015q4/bin/arm-none-eabi-gcc

13. 多重stm32Cube库
	(两个位置的库暂留，待统一再删除不常用的)
	1. /home/wmt/STM32Cube/Repository/
	2. /opt/STM32Cube

12. stm32开发with Matlab
-	[STM32-MAT](https://www.st.com/en/development-tools/stm32-mat-target.html)
	1. 在matlab中集成stm32-mat
		cmd: pathtool (matlab search path)
	2. 存储pathdef.m ->　matlab目录
	2. 新建model
		配置参数：code generation -> System target: stm32.tlc
		先择setm32.tlc -> ok后，却出现了：
			警告：请把stm32包添加到Matlab的default路径中，用toolpath命令．
		设置stm32cubeMx的安装路径：/usr/local/STMicroelectronics/STM32Cube/STM32CubeMX
		Error: 执行PostApplyCallback of the target stm32.tlc:路径没有找到．
	 
11. stm32流程
	(1)烧写程序 
		st-flash write a.bin 0x8000000
	(2)编写.gdbinit
	(2)开启openocd server
		openocd -f interface/stlink-v2.cfg -f target/stm32f4x_stlink.cfg
	(3)开启gdb
		arm-none-eabi-gdb uart.elf	
10. 列出一些openocd的基本用法，并实验
<<OpenOCD User’s Guide >> 放在本地文摘中"openocd_debug.pdf" [网络](http://openocd.org/doc/pdf/openocd.pdf)
	Open On-Chip Debugger:  for release 0.10.0+dev 19 November 2018

1. 在demo stm32中，学会怎么设置项目的make
2. 想办法提出make文件（keil的），然后移植到linux下Make
3. [stm32-cmake](https://github.com/ObKo/stm32-cmake) 学习笔记见 st cmake
4. [A demo project of FreeRTOS running on a STM32F4 Discovery board.](https://github.com/wangyeee/STM32F4-FreeRTOS) (利用现存的的例子，结合以上cmake工具)
5. 利用以上freeRTOS项目剖析并对照<<stm32>>书，和以上第３项cmake
	目的是编译这个项目没错，这个项目的东西，在CMAKE中是怎么对应的；并可以理解各部分的作用．
	编译　STM32F4-FreeRTOS-> 只需编辑makefile设定编译器的路径．-> make
6. openocd 不能运行
　Can't find openocd.cfg
7. 6项不行，从官网软件突破jlink for linux 
	有两份文档(JLink JFlash)已经下载(在文摘中)
-	rpm软件　[JLink_Linux_x86_64.rpm](https://www.segger.com/downloads/jlink/JLink_Linux_x86_64.rpm)
	安装之后，知识点滴13
	  		
8. 要转CMSIS-DAP OPENOCD [那些事][https://blog.csdn.net/m454078356/article/details/78986205]

9. 野火CMSIS-DAP, 给的openOCD
  https://zhidao.baidu.com/question/1047512972598364779.html?spm=a1z09.2.0.0.393b2e8djKSl0A
[ARM官方][https://os.mbed.com/handbook/CMSIS-DAP]

10. 第８项＂那些事＂提到了一个配置文件ocd-stm32.cfg ---它是一个脚本
	它指定了interface和transport (界面卡和传输端口)
		cmsis-dap	jtag
	再指source(源)为target/stm32f1x.cfg ---它是一个脚本

## 知识点滴
21. 面向对象的lw_oopc怎样才能完成对象指针类型的转换
	 SUPER_PTR(fish, Animal); fish小类/Animal大类-> 返回一个可被操作的，等同大类的　fish实例指针

20. 指针参数前为什么要加const关键字 const char\* str
	如果函数参数是指针，且仅作输入用，则必须在类型前面加上const，以用来防止该指针在函数体内被意外修改。

19. c语音的static作用
18. x-cube-mems1/application 5.2.1 device 是什么PACK？会在pinout&configuration中
	运动软件算法库: X-CUBE_MEMS1
	麦克风采集处理软件库: X-CUBE-MEMSMIC1

17. grt.tcl ert.tcl 区别
	grt.tcl general 一般用来做sil测试，看生成软件的架构功能正确性
	ert.tcl embeded 针对器件对代码优化 
16. 启动openocd　server
	openocd -f interface/stlink-v2.cfg -f target/stm32f4x_stlink.cfg
15. stlink-v2 : 0483:3748
14. st-link v2 连接OPENOCD与STM32
13. J-link 软件结构 --所买野火不支持　(将不会用jlink,要把这些删除)
	cat /etc/udev/rules.d/99-jlink.rules
	tree /opt/SEGGER/JLink	
	ll usr/bin/J*
	locate jlink (可看到stm32 用到jlink的demo)
12. FreeRTOSConfig.h
	rtos下的示例子项目，对这个文件的配置可能不相同
11. FreeRTOS API的利用 p80 子项目：rtos/blinky2(rtos子目录有很多可以学)
	make clobber & make & make flash
10. FreeRTOS (以下的概念须了解)
	• Multi-tasking and scheduling
	• Message queues
	• Semaphores and mutexes
	• Timers
	• Event groups
9. GPIO API
	#include <libopencm3/stm32/rcc.h>
	#include <libopencm3/stm32/gpio.h> 
8. make flash
	在子项目中也可以这样，当前的BIN文件烧入FLAS8. make flash
7. objcopy -Obinary 
	把可执行的elf文件转成img文件（bin文件）-> to flash
6. 在子文件夹中也可以make
	产生了elf bin文件
	elf文件是可执行文件，
	arm-none-eabi-objcopy -Obinary miniblink.elf miniblink.bin
5. make clobber 
	是把上一次make命令生成的文件或目录清除掉,效果比make clean更严格。
	清除之后，只剩下一个c文件和Makefile
4. st-flash (write / read / erase)
	执行烧写
	st-flash write a.bin 0x8000000
3. make命令
	执行build
2. Makefile.incl
	PREFIX ?= arm-none-eabi
	为gcc设定正确的前缀
1. Project.mk STM32项目中rtos子目录中
	改变FREERTOS 对应的版本号(目录)
	目的让make file可以正确的工作


### stm32 函数
1. SysTick_Config()
	初始化systick; 打开systick; 中断并设置优先级; 返回一个0代表成功或1代表失败
	systick定时器是24位的递减计数器	
2.  void HAL_SYSTICK_Callback(void)
	SysTick的回调

