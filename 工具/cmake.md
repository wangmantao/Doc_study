# cmake 用法
[Makefile 生成d文件 依赖文件及相关选项说明](https://blog.csdn.net/QQ1452008/article/details/50855810)

## 灵感

11. pkg-config --libs -cflgs xxx
10. include 在当前位置插入另外的makefile文件，如果文件不存在就报错（想不报错，用-include)
9. MAKECMDGOALS 环境变量，命令行参数指定的终极目标列表，没有通过参数指定终极目标时此变量为空。该变量仅限于用在特殊场合(比如判断)
8. make文件，定义头文件的位置　
	CFLAGS=-I/home/develop/include
	gcc $(CFLAGS) 
7. 这个项目用的是cube库，可以用其它库吗？
	印度人写的stm32的项目用的库是： libopencm3
	uwb评估板用的库是：		CMSIS

5. stm32-cmake 项目的利用
	最佳体验是不改项目中的任何文件，自己新增文件达到使用目的．如变量定义在一个文件，让cmaek调用或批处理

4. git项目: stm32-cmake 
	除了用cmake 带上一些参数外，还要配置要用到的头文件在config.h中．见以下＂出坑经验＂

3. git项目: STM32F4-FreeRTOS　重点目录略看

2. 起因2
	用于stm32项目的构建 (网址见st freertos)

1. 起因1
　　clojure项目怎么会有c文件，两个文件就能用到了cmake.
　　cmake . 查找什么文件就能make了?
 　cmake是什么工具，是制作make文件的吗？它的首查脚本文件是什么文件



## 知识点滴
1. cmake 引用头文件的配置在config.h中，好像不用手动改
2. Makefile的PROJECTS 变量可以写多个项目名字，意为都进行make

## 资源
2. [stm32cube F系列查找](https://www.st.com/content/st_com/zh/search.html#q=stm32cube-t=tools-page=1))
1. [stm32cube F1各种资料](https://www.st.com/content/st_com/zh/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-mcu-packages/stm32cubef1.html)


------------------------------------------------------------------------------------------------------------
## 出坑经验
### STM32F4-FreeRTOS 编译过程
	build:	中途的库文件
	binary: 可执行文件
	Libraies: 
		CMSIS / STM32F4xx_StdPeriph_Driver 
		CMSIS: 为Device F4XXX的头文件
		外设驱动：stm32f4xx前缀的h/c文件
		syscalls.c  
	hardware: 
		*_it.h *_it.c system_*.c
		startup_stm32f4xx.s (s文件)
			GCC工具链的向量表
			-- STM32F40xxx/41xxx Devices vector table for GCC based toolchain 
	config:
		FreeRTOSConfig.h / stm32f4xx_conf.h
	用法：只是在makefile中指定toolchain的位置

### stm32-cmake 编译不过
-	[可参见：](temp/test/stm32-cmake/README.md)
	它首先要设置cmake脚本吗？－－见起因１
	(答：要脚本，在cmake时指定的主脚本入口为: cmake -DCMAKE_TOOLCHAIN_FILE= ../*/gcc_stm32.cmake)

* 目录:
	package: f1xx or f4xx 的CMSIS及stdPeriph库 (这个目录什么用？)
		产生package(rpm)的功能有点意思，目的是用cmake产生cmsis和外设的库
-		用到了[fpm项目](https://github.com/jordansissel/fpm)	
	cmake: gcc_stm32.cmake 配制工作链和库
		以下好多cmake文件，不知怎么互相调用
* 用法：１．配置工具链和库 -> 已经在**cmakes/gcc_stm32.cmake**中设了TOOLCHAIN_PREFIX 
		2. 设置变量STM32_CHIP (暂时不设看报错)
-		3. 设置变量STM32Cube_DIR -- 暂不 (实为：/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0)
		4. 设置CMAKE_MODULE_PATH -- 暂不
* 命令：
	cmake -DSTM32Cube_DIR=/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0 -DSTM32_CHIP=STM32F103C8  -DCMAKE_TOOLCHAIN_FILE=../cmake/gcc_stm32.cmake -DCMAKE_BUILD_TYPE=Debug ./

* 运行结果是：
-- STM32F103C8 is 103xB device
-- Found **CMSIS**: /opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/CMSIS/Device/ST/STM32F1xx/Include;/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/CMSIS/Include  
-- Found **STM32HAL**: /opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Inc  
-- No linker script specified, generating default
-- STM32F103C8 has _64KiB of flash_ memory and _20KiB of RAM_
-- Configuring done
-- Generating done
CMake **Warning**:
  Manually-specified variables were not used by the project:
    CMAKE_TOOLCHAIN_FILE -----为什么没被用上? (原因是本目录有个cmakeList.txt)
-- Build files have been written to: /home/wmt/temp/test/stm32-cmake/stm32-blinky_test

* 用make执行编译
	错误
* 改用项目中的原目录名实验（不是stm32-blinky_test)
[wmt@cf-nx2 stm32-blinky]$ cmake -DCMAKE_MODULE_PATH=./CMakeFiles -DSTM32Cube_DIR=/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0 -DSTM32_CHIP=STM32F103C8  -DCMAKE_TOOLCHAIN_FILE=../cmake/gcc_stm32.cmake -DCMAKE_BUILD_TYPE=Debug ./
-- No TOOLCHAIN_PREFIX specified, using default: /opt/gcc-arm
-- No TARGET_TRIPLET specified, using default: arm-none-eabi
-- No STM32_FAMILY specified, trying to get it from STM32_CHIP
-- Selected STM32 family: F1
-- The C compiler identification is GNU 7.3.1
-- The CXX compiler identification is GNU 7.3.1
-- Check for working C compiler: /opt/gcc-arm/bin/arm-none-eabi-gcc
-- Check for working C compiler: /opt/gcc-arm/bin/arm-none-eabi-gcc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /opt/gcc-arm/bin/arm-none-eabi-g++
-- Check for working CXX compiler: /opt/gcc-arm/bin/arm-none-eabi-g++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- The ASM compiler identification is GNU
-- Found assembler: /opt/gcc-arm/bin/arm-none-eabi-gcc
-- STM32F103C8 is 103xB device
-- Found CMSIS: /opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/CMSIS/Device/ST/STM32F1xx/Include;/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/CMSIS/Include  
-- Found STM32HAL: /opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Inc  
-- No linker script specified, generating default
-- STM32F103C8 has 64KiB of flash memory and 20KiB of RAM
-- Configuring done
-- Generating done
-- Build files have been written to: /home/wmt/temp/test/stm32-cmake/stm32-blinky
* make 仍然出错
GPIO_SPEED_HIGH--不认    ./STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Inc/**Legacy/stm32_hal_legacy.h**
GPIO_SPEED_FREQ_HIGH --认 /STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Inc/stm32f1xx_hal_gpio.h 

* config.h 加入了相应的头文件
[  7% ] Building C object CMakeFiles/stm32-blinky.dir/main.c.obj
[ 15% ] Building C object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/CMSIS/Device/ST/STM32F1xx/Source/Templates/system_stm32f1xx.c.obj
[ 23% ] Building ASM object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/CMSIS/Device/ST/STM32F1xx/Source/Templates/gcc/startup_stm32f103xb.s.obj
[ 30% ] Building C object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal.c.obj
[ 38% ] Building C object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_gpio.c.obj
[ 46% ] Building C object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_gpio_ex.c.obj
[ 53% ] Building C object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_tim.c.obj
[ 61% ] Building C object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_tim_ex.c.obj
[ 69% ] Building C object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_cortex.c.obj
[ 76% ] Building C object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_pwr.c.obj
[ 84% ] Building C object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_rcc.c.obj
[ 92% ] Building C object CMakeFiles/stm32-blinky.dir/opt/STM32Cube/STM32Cube_FW_F1_V1.6.0/Drivers/STM32F1xx_HAL_Driver/Src/stm32f1xx_hal_rcc_ex.c.obj


