## 知识点滴
50. sudo 不要密码
    sudo方式不需要输入密码:echo "admin" | sudo -S service tomcat7 stop
    (显示的密码，给后面的管道)

49. 怎么开机时运行守护程序deamon
    见28.  开机启动

48. syslog
    c语言可以用syslog函数，向系统日志添加日志。
    但fedora没有该服务，可以yum install rsyslog, 并systemctl start rsyslog
    这样在/var/log/messages 文件中可以看到syslog函数发出的日志信息了。

47. Linux 守护进程deamon
    int deamon(int nochdir, int noclose)
        (nochdir =0: 意改为根目录；noclose=0 :意为关闭所有文件描述符)
    例：
            include <sys/types.h>
            include <sys/stat.h>
            include <stdlib.h>
            include <stdio.h>
            include <fcntl.h>
            include <unistd.h>
            include <linux/fs.h>
            int main(){
                    daemon(0,0);
                    //下面可以写自己的操作...
                    while(1){
                            sleep(1);
                    }   
                    return 0;
            }
    ---------------
    gcc –g –o test test.c 
    ps -ef

46. [通过新机安装cp210驱动模块的总结]
        1. 编译
            因为要make module, 所以要用到内核源码（/usr/src/kernel/ver....../)
                没有源码，可以能过yum install kernel-devel-$(uname -r) (19.项类似)
            vcp(pj vcp可找到)内的Makefile配置kernelDir时，所指的目录要有Makefile文件(用于Make内核的)
        2. 模块放置path (vcp内的readme.md有讲)
        3. 模块自动加载 (vcp内的readme.md有讲)

45.  取消屏幕锁屏(屏保 待机)
    System->setting->privacy.
    
44. 风速表的数字解析
    BYTE1	BYTE2	BYTE3	BYTE4	BYTE5	BYTE6	BYTE7	BYTE8

    BYTE3-5	main_4.data.H	
	main_4.data.L	
	main_4.X10   (10^X10)	

    main_4.data表示LCD是4个最大数码显示的值
     X10是一个char型数据(有符号),表示显示数码的数量级			
    当main_4.data=1543,X10=-2时,表示的数据为:  main_4.data * 10 ^ X10= 15.43			
    当main_4.data=1543,X10 =2时,表示的数据为:  main_4.data * 10 ^ X10= 1543*100			
    (LCD上显示为"1543" " X100 " ) 			

44. hid设备的读写解析
   1. 建立一个256的buffer
   2. 全部置０
   3. 最前两位置 0x01 0x81
   4. 向hid设备读数据到buffer, 可能会无结果，但不会导致读取阻塞
   5. 向buffer前面置5个字节 0x02 a0 0a 00 00
   6. 用一个hid_send...命令发个特性报告(5项内容)到device
   7. 此时特性报告不能发送
   8. 关于feature report 都不能获得报告 
   9. hid_write没有出现错误
   10. 第一次write 0x01 0x80 (改变LED)
   11. 第二次write 0x01 0x81 (询问状态)
   12. read刚请求的状态，如果用非阻塞式read, 用(hid_set_nonblocking函数) 
   13. 在buffer中处理读取的结果

43. hid设备(fan 风扇)
          Device Found
          type: 1234 5678
          path: /dev/hidraw3
          serial_number: 
          Manufacturer: 
          Product:      (null)
          Release:      1
          Interface:    0

42. hid usb 库的支持 (hidapi 在myProj/wmt)
    git clone https://github.com/signal11/hidapi.git
    ./bootstrap
    ./configure
    make 
    make install　

    得到库:
       /linux/.libs/libhidapi-hidraw.so                                                                  │
        ./libusb/.libs/libhidapi-libusb.so    　

    安装在：
        /usr/local/lib
    If you ever happen to want to link against installed libraries    
in a given directory, LIBDIR, you must either use libtool, and    
specify the full pathname of the library, or use the '-LLIBDIR'   
flag during linking and do at least one of the following:         
   - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable     
     during execution                                             
   - add LIBDIR to the 'LD_RUN_PATH' environment variable         
     during linking                                               
   - use the '-Wl,-rpath -Wl,LIBDIR' linker flag                  
   - have your system administrator add LIBDIR to '/etc/ld.so.conf
                                                                  
See any operating system documentation about shared libraries for 
more information, such as the ld(1) and ld.so(8) manual pages.   

41. 如何查看内存有无泄漏
40. Aborted (核心已转储) 查看堆栈调用，找出错误源
    journalctl -a (一法：看系统日志, 无细研)    
    (二法)
    coredumpctl这个工具，该工具可以在调试:
        无论是终端用户产生的core;
        还是后台服务产生的core.
        使用方法为：。
        调试最近一次coredump & 进入gdb调试模式: coredumpctl gdb
        调试其他程序:coredumpctl gdb pid

39. 串口udev 设置rule (tty属于“dialout”组别)
    /etc/udev/rules.d/70-ttyusb.rules
    /etc/udev/rules.d/70-ttyusb.rules

38. 自己做一个静态库 并　调用
 g++ -pthread -I../include main.cc ../build/libmyserial.a ../build/libserial.a
    libserial.a 是依赖库
    libmyserail.a 是自定义库
    main.cc 是调用库的程序

37. c++ 多线程的编译
    cmake 添加以下语句 (在CMakelist.txt)
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -pthread")

36. cmake的用法 (好教程：https://blog.csdn.net/kai_zone/article/details/82656964)
    1. 在项目的根目录建 CMakelist.txt (内容在后)
    2. 根目录下有include src build
    3. 在build下执行 cmake
    -----  未完 ----
    4. 会自动生成Makefile, 就在build目录下进行make
        (源文件变更后，直接make)

        [CMakelist.txt 写法]
        cmake_minimum_required(VERSION 2.8.3)
        project(my_project)

        include_directories(include)

        # 设置源的变量
        set(serial_SRCS src/serial.cc include/serial/serial.h include/serial/v8stdint.h)

        if(UNIX)
            list(APPEND serial_SRCS src/impl/unix.cc)
        else()
            list(APPEND serial_SRCS src/impl/win.cc)
        endif()

        ## 将变量目录中的源文件编译为共享库"serial"
        add_library(serial ${serial_SRCS})

        if(UNIX AND NOT APPLE)
            target_link_libraries(serial rt)
        endif()

        ## Build your executable
        add_executable(testBin src/test.cc)

        ## 指定可执行文件testBin需要链接serail这个库
        target_link_libraries(testBin serial)

35. find 查找可执行文件
    find -type f -perm -111
34. 夸平台串口库
    https://github.com/wjwwood/serial/issues/52

33. vsftpd vsftp 553 不能put文件
    setsebool -P ftpd_disable_trans 1 (无此项)
    getsebool -a|grep ftp
        ftpd_anon_write --> off
        ftpd_connect_all_unreserved --> off
        ftpd_connect_db --> off
        ftpd_full_access --> off
        ftpd_use_cifs --> off
        ftpd_use_fusefs --> off
        ftpd_use_nfs --> off
        ftpd_use_passive_mode --> off
        httpd_can_connect_ftp --> off
        httpd_enable_ftp_server --> off
        tftp_anon_write --> off
        tftp_home_dir --> off
   sudo setsebool ftpd_full_access on --> 解决 lftp wmt@ip 可以上传

32. 设置了.bash_profile 导至进不了系统
    live usb 启，mount 硬盘，改回.bash_profile
    https://blog.csdn.net/a19860903/article/details/54894534

31.
    还有一种就是可以在用户目录下的一个.bash_profile
这个文件里加入要启动的文件命令或文件启动程序所在地址，完整的也可以开机就启动

30.桌面应用 applicat
    Fedora的应用信息存放在/usr/share/applications (好多desktop)
    /etc/xdg/autostart <- desktop 放进来就可以启动了


29. 用qt控制启动程序的开关
     java -jar clj-pc-0.1.0-SNAPSHOT-standalone.jar > /dev/null 2>/dev/null &
    echo $!  29468 (表示下上一个后台进程id $!)
    #可以使用nohup,即使关闭终端，程序也不会停止
    nohup bin/kibana  > kibana.log 2>&1 &

28.  开机启动
    profile     登录后运行
    /ect/rc.d/rc.local 登录前运行 (chmod +x this)
        #!/bin/sh -e
        #rc.local
        #This script is executed at the end of each multiuser runlevel.
        #Make sure that the script will "exit 0" on success 
        #or any other value on error.
        java -jar file.jar &    -> 后台运行
        exit 0

    续：以上配置后开机不执行(再续：通过看开机启动脚本，发现rc.local启动失败)
    在/etc/rc.d/rc3.d 建立连接 (可以不做)
    sudo ln -s rmtkeyboard /etc/rc.d/rc.local (续启动失败)

    启动用户界面后自动运行
    /home/wmt/.config/autostart/启动脚本.sh
    sudo方式不需要输入密码:echo "admin" | sudo -S service tomcat7 stop

    最终成功的方法：
    1. 先建立一个启动程序的脚本（可用sudo)
    2. 在.config/autostart/中 　建立一个desktop文件，其它的exe指向脚本　

27. vsftp 
    systemctl start vsftpd
    iptables -F 

26. 创效的ip
    192.168.5.2
    路由192.168.5.1
    DNS 192.168.5.1

25. 安装c c++ 编译环境
    sudo yum install gcc gcc-g++ cmake

24. 取消用户密码

23. 登录密码环
    未解除:
    rm -rf /home/wmt/.local/share/keyrings 

22. 自动登录(并指定用户名)
    #vi /etc/gdm/custom.conf
    在最后添加以下内容：
    [daemon ]
        TimedLoginEnable=true    允许超时自动登录
        TimedLogin=jack      自动登录的用户为jack
        TimedLoginDelay=3          超时时间为3秒，可不设

        AutomaticLoginEnable=true
        AutomaticLogin=cx (root) (wmt)

    https://ask.fedoraproject.org/en/question/48956/how-to-open-fedora-without-user-password/

22. 脚本自动运行
        将要运行的脚本放入/etc/profile.d/,
        也可以在此目录下新建脚本调用其他位置的脚本，如：
        /etc/profile.d/load-xxx.sh

21. ldd 找不到链接库
    库的路径在这里设置 /etc/ld.so.conf  (它执向./ld.so.conf.d/目录)

20. 为了传文件，ecs 做了改变
    /etc/ssh/ssh_config  :StrictHostKeyChecking ask (新的no)
    将来要改回 systemctl restart sshd

19. 如果编译驱动时，没有build
    /lib/modules/xxxxx.xxx 没有
    需要安装内核包：yum install kernel-devel (不是本系统的内核开发包)
     正确方法是：
     sudo yum install "kernel-devel-uname-r == $(uname -r)"
    
18. 旧电脑显卡驱动问题
    https://www.cnblogs.com/noxy/p/9560911.html

17. postgresql 安装与启动 
    yum install postgresql postgresql-server -y
    安装完后要初始化　/usr/pgsql-9.6/bin/postgresql96-setup initdb
                    (本机为 /usr/bin/postgresql-setup initdb)
                    /var/lib/pgsql/data  不为空，删掉　－－此步就是初始文件
    将这个数据库服务开机启动
            sudo service postgresql start (启动有效)
    执行本次启动
            sudo systemctl start postgresql@
    通过端口，看服务有没开启
        netstat -tlunp | grep 5432
    ---------------------------------------
    ps aux | grep postgres (可看数据库进程，被什么角色的用户进程开的)
    sudo su - postgres      (改变到这个用户下操作，我也不知道本机有没这个用户)
    (postgres home 在　/var/lib/pgsql ) 要以放写好的数据库脚本


16. 查看当前有哪些组
    groups

15. 查找软件是否安装　＆　安装位置
    rpm -qa |grep 软件
    rpm -qal |grep 软件　　－》　看位置

14. fedora creater 的下载
    dnf install liveusb-creator

13. 测试用电脑问题：
    (后)　移动硬盘增设了　联想台式机分辨率的设置脚本~/bin/temp1080.sh
    禁用XWAYLAND
	/etc/gdm/custom.conf
	#WaylandEnable=false
	
	以下自己的笔记本可行:
	cvt 1920 1080
	xrandr --newmode "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
	xrandr --addmode VGA-1 "1920x1080_60.00"

	https://blog.csdn.net/KjfureOne/article/details/52840107 可以改　rate

    == 问题2: 开发电脑文件 - > 测试电脑
    1. ~/.m2
    2. /etc/udev/rules.d/stm32.rules
    3. /home/wmt/temp/vcp   --cp210x 驱动源码 (添加了gdm万用表的识别)
        (改位置:/home/wmt/myProj/wmt/vcp)
        源码编译后，ko放置：/lib/modules/4.13.12-200.fc26.x86_64/kernel/drivers/usb/serial/cp210x.ko.xz 
    4. /etc/sysconfig/modules/cp210x.modules  -- 驱动模块的开机自安装
    5. lein  
    6. /usr/java/jdk1.8.0_201-amd64/lib/amd64/librxtxSerial.so  (这个和m2中的java接口相对应) (缺少rxtxParallel.dll和rxtxSerial.dll for Windows)
    System.out.println( System.getProperty("java.library.path"))

user=> (println (System/getProperty "java.library.path"))
        /home/wmt/Qt5.10.0/5.10.0/gcc_64/lib:/home/wmt/Qt5.10.0/5.10.0/gcc_64/lib::/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
        
       java -jar clj*.jar 找不到串口库so文件。
       设置：.bashrc LD_LABRSRY_PATH https://www.cnblogs.com/LiuYanYGZ/p/6110822.html
    7. /usr/bin/disp.sh (在测试电脑中的文件，自动调整分辨率)
	
12. 设备的id
	Bus 003 Device 113: ID 1366:0105 SEGGER 
	Bus 003 Device 122: ID 0483:5740 STMicroelectronics STM32F407
	udev 的帮手
	udevadm info -a -n /dev/
e
11. linux 启动加载modules	
	/etc/sysconfig/modules 添加脚本 （注意这个可执行脚本扩展名要是modules, 并要是可执行文件)
	https://blog.csdn.net/hxpjava1/article/details/79703609
	modinfo -F /lib/modules/4.13.12-200.fc26.x86_64/kernel/drivers/usb/serial/cp210x.ko.xz cp210x
	(此处的ko文件多添加了xz后缀，不然modinfo cp210x 找不到)
    (还注意以下多一步骤)

	-- 无用 --
	Linux: How to load a kernel module automatically at boot time
	https://www.cyberciti.biz/faq/linux-how-to-load-a-kernel-module-automatically-at-boot-time/
	module位置:/usr/lib/modules/4.13.12-200.fc26.x86_64/kernel/drivers/usb/serial/cp210x.ko
	编写配置文件放在 /etc/modules-load.d
	echo cp210x > /etc/modules-load.d/cp210x.conf	

10. 最终解决串口发送固纬指令问题
	 (defn get-bytes [^String s ^String charset] (.getBytes s charset))
	 (write port (get-bytes "SYST:SER?\r\n" "UTF-8"))


9. 最终解决rxtx的lock权限
	主要：http://blog.thunderbits.org/tag/how-can-i-use-lock-files-with-rxtx/
		把我当前用户wmt,添加到组：uucp lock tty
		改变/run/lock 目录的所属: root:lock    (每次重启会变吗？)
			开机后（又是root:root)
		让/run/lock 目录给所属者有写的权限: g+w (这个也要再做一遍)
		
	次要：(不知有没有用)
		改了/etc/group
		安装了uucp
		添加了uucp用户属wmt组
		

8. 每次加载　~/temp/vcp/cp210x.ko
. 每次加载　 sudo chmod o+rw /dev/ttyUSB0 

7. 解决linux下java读取串口之权限问题 No permission to create lock file
	https://www.xuebuyuan.com/2187596.html  (改/etc/group文件)
	(list-ports) -- relp 中此命令的问题

6. 串口底层库
	/usr/local/MATLAB/R2018a/bin/glnxa64/librxtxSerial.so
	cp 到　JAVA_HOME/amd64/下:(-----以上的库太旧)

	要用到两个文件：
	1. jar
	2. so
	http://rxtx.qbang.org/pub/rxtx/rxtx-2.2pre2-bins.zip
	(这个压包里有以上两个文件的需求，但最好用以下方法下载m2依赖)
	https://github.com/samaaron/serial-port
	这个项目的project.clj 里的independcy 可自动下载m2依赖

5. 终端窗口中操盘串口
	echo cmdStr > /dev/ttyUSB0  向串口发送命令
	sudo cat -v < /dev/ttyUSB0    一直获取串口的返回内容

	sudo chmod o+rw /dev/ttyUSB0  设置一般权限
	sudo stty speed 115200 > /dev/ttyUSB0 

4. fedora 源文件在哪、
	/usr/src/kernels/4.13.

3. 查看驱动模块是否存在(cdc_acm 没有解决固纬的问题)
	 modinfo cdc_acm
	sudo modprobe -r cdc_acm
	sudo modprobe cdc_acm vendor=0x2184 product=0x0300 （加GDM-8341)
		

2. 没有/proc/usb/devices
	 实在： /sys/kernel/debug/usb/devices (可以看到驱动什么)
	（lsusb -v 则看不到关联的驱动)
	固纬万用表显示没有驱动
	P:  Vendor=2184 ProdID=0030 Rev= 1.00
	S:  Manufacturer=Silicon Labs
	S:  Product=GDM834X VCP PORT
	S:  SerialNumber=GES885640
	I:* If#= 0 Alt= 0 #EPs= 2 Cls=ff(vend.) Sub=00 Prot=00 Driver=(none)
[解决](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers) 本地源码在 ~/temp/vcp (添加id)->make -> insmod icp20x.ko (rmmod icp20x.ko )
	 If#= 0 Alt= 0 #EPs= 2 Cls=ff(vend.) Sub=00 Prot=00 Driver=cp210x
	
	
1. 系统日志减
sudo journalctl --vacuum-time=1years   (1week)



# Linux 操作系统笔记　
[fedora28 32位] (https://dl.iplaysoft.com/files/4726.html)
这个文件里加入要启动的文件命令或文件启动程序所在地址，完整的也可以开机就启动


# Linux 串口的操作功能　
    screen -ls 查看当前会话的状态
    screen -d -m /dev/tty**
    对文件描述符 file-descriptor 执行close.退出程序，但没有退出screen.
    reattach 重新连接 scrren -r 
    agettty 是一个tty 监测程序
    lsof -i:端口号
    
    sudo python reset_usb.py cp210x
    读数据出错，可能需要运行fsck 检难文件系统是否有问题
    

    /dev/bus/usb/总线号/设备号，可以用lsusb -t 查看相关信息
    怎么查看某个串口的总线号和设备号
        dmesg |grep ttyUSB0 
            返回:usb 3-2: FTDI USB Serial Device converter now attached to ttyUSB0
                (总线2,设备2)


    > 原代码网上有(myProj/wmt/usbreset)
    ./usbreset /dev/bus/usb/003/008 (3&8 获取方法)


    > c代码方法  (myProj/wmt/usbreset)
    c代码得到bus号，dev号
    https://stackoverflow.com/questions/20249418/find-bus-number-and-device-number-with-device-file-symlink

    > 命令方法：
    udevadm info -a -p  $(udevadm info -q path -n /dev/ttyUSB0)
    udevadm info -a -p /sys/bus/usb-serial/devices/ttyUSB0/ |grep busnum
    udevadm info -a -p /sys/bus/usb-serial/devices/ttyUSB0/ |grep busnum

    echo /dev/bus/usb/`udevadm info --name=/dev/ttyUSB0 --attribute-walk | sed -n 's/\s*ATTRS{\(\(devnum\)\|\(busnum\)\)}==\"\([^\"]\+\)\"/\4/p' | head -n 2 | awk '{$1 = sprintf("%03d", $1); print}'` | tr " " "/"
        返回：/dev/bus/usb/003/010 
        命令解释: https://askubuntu.com/questions/184526/how-to-get-bus-and-device-relationship-for-a-dev-ttyusb

    > 扩展成一句命令
    echo /dev/bus/usb/`udevadm info --name=/dev/ttyUSB0 --attribute-walk | sed -n 's/\s*ATTRS{\(\(devnum\)\|\(busnum\)\)}==\"\([^\"]\+\)\"/\4/p' | head -n 2 | awk '{$1 = sprintf("%03d", $1); print}'` | tr " " "/" |xargs -i usbreset {}

    

# 安装一台fedora过程
    1. U盘安装(设置root无密码登录) 
    2. yum install gcc cmake opengl (以下备注1)
    3. 下载安装qt5.0  (百度: qt5.10 下载)
    4. postgresql 安装/初始化/启动(备注2) (postgresql-setup initdb)　（systemctrl enable/start postgresql)
    5. 开发机导出数据库() 部署机导入数据库()

        备注1:
        yum install gcc gcc-g++ cmake vim
        yum install mesa-libGL-devel mesa-libGLU-devel //这两个是opengl核心库
        yum install freeglut-devel //OpenGL Utility ToolKi
        
        备注2:
        yum install postgresql-server.x86_64 postgresql-contrib.x86_64

### 安装项目
    hidapi 
        1. git clone
        2. yum install libudev-devel libusb-devel
        3. ./autoboot ./configure .. make make install
        4. install 位置
            /usr/local/include/hidapi
            /usr/local/share/doc/hidapi
            /usr/local/lib/libhidapi-*./a(so) 用的是so
            /usr/local/lib/pkgconfig


