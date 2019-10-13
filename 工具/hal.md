
# HAL库使用规则
1. 函数中的：HAL_"_做前缀. 只需要记后面的部分

2. 函数分类
	回调 Callback	要想学单片机的功能，那就找有哪些Callback 
	中断 _Start_IT	知道哪些中断

3. 函数记 
	GPIO_读
	GPIO—写

4. 名词
定时器相关：
其它：
DMA的定义
直接存储器存取（Direct Memory Access，DMA）是计算机科学中的一种内存访问技术。它允许某些电脑内部的硬体子系统（电脑外设），可以独立地直接读写系统存储器，而不需绕道 CPU。在同等程度的CPU负担下，DMA是一种快速的数据传送方式。它允许不同速度的硬件装置来沟通，而不需要依于 CPU的大量中断请求。

D
5. 认知HAL库 
	GPIO对象（结构）
	HandleTypeDef (句柄类型定义)

6. 外设学习
	timer : 句柄.初始化.Prescaler/CounterModer/Period/ClockDivision
	