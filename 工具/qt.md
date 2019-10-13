# QT用法
## Document
    Qt Design Studio Manual 1.3
    https://doc.qt.io/qtdesignstudio/studio-getting-started.html

# 灵感

## 知识点滴
39. qt design studio最佳实验步骤
    1. 项目管理
        建立UI项目，使用git, 导入设计，将ui项目转换为应用程序
        重点要学会git, 及转换
    2. 创建ui
        创建组件，管理项目层次结构，指定项目属性，制作动画，添加连接，加法国
    我要利用以上的步骤完成作业项目的设计

38. Qt Design Studio运行方式
    它安装在home/wmt/qtdes.... 下面,但要运行它需要sudo 
    临时对策是:在bin下面 qds.sh

37. Offline Installers 离线安装包
    QT:
    https://www.qt.io/offline-installers

    Qt Design Studio:
    https://download.qt.io/official_releases/qtdesignstudio/1.3.0/
    https://www.qt.io/cn/ui-framework  (中国)

36. qml 结合外部软件设计
    Qt Design Studio 1.2最值得注意的是Qt Bridge for Sketch。 
    这允许您将设计无缝导入Qt Design Studio

35. 程序重启自己
    execv 函数  调用的进程会覆盖掉主进程. (unistd.h)
    引申：exec  不退出主进程,也不建立新的进程。
    (其它还有５个类似)
    以上属于范畴：在进程中运行新代码

    原形:int execv(const char *path， char *const argv[]);
    execv()函数函数用来执行参数path字符串所指向的程序(第一个)
    (第二个为数组指针)维护的程序参数列表，该数组的最后一个成员必须是空指针.
    说白了，第一个是命令，第二个是命令的参数

    重启自自己的示例：
    char *exec_argv[] = { argv[0], buf, 0 };
    execv("/proc/self/exe", exec_argv);
    
    知识点：
    /proc/self/目录，这个目录比较独特，
    不同的进程访问该目录时获得的信息是不同的，
    内容等价于/proc/本进程pid/。
    进程可以通过访问/proc/self/目录来获取自己的系统信息，
    而不用每次都获取pid。
    用法：proc/Self/exe: 指向当前可执行文件的路径

34. 关于qt debug信息日志
    1. 日志最好写入文件，不用人工现场看标准输出
    2. 日志每行能约定显示时间；类；函数

33. 关于线程的暂停
    c# thead 靠等信号来阻塞线程，而控制信号类有AutoResetEvent ManualResetEvent
        AutoResetEvent.WaitOne();
    QT QThread 靠互斥来阻塞线程，用法：
        关键道具，一把锁和一个状态标记.
    c++
        C++ 的 std::condition_variable 和 std::mutex 

    通过 mutex 把有严格时序要求的代码保护起来，
    同时把 wakeAll() 也用同一个 mutex 保护起来，
    这样能保证：一定先有 wait() ，再有 wakeAll()，
    不管什么情况，都能保证这种先后关系，而不至于摆乌龙。

32. qmake中使用opencv 
     项目名.pro中添加:
     CONFIG += link_pkgconfig
     PKGCONFIG += opencv

31. QTimer 简单应用
     QTimer::singleShot(1, &Sender::myFunction1);

30.  How to connect static signal from different class to slot?
    https://stackoverflow.com/questions/54154656/qt-how-to-connect-static-signal-from-different-class-to-slot
    关键点：
        const bool connected =  QObject::connect(&Sender::instance(), &Sender::mySignal, &w, &MainWindow::mySlot);
    这个连接要在main.cpp中完成，因为slot是mainWindow中，mainWindow又是在main.cpp中实例化的。

29. 静态成员发射信号
     signals:
    emitFunction(int);
    private:
    static int callback(int val)
    {
        /* Called multiple times (100+) */
        theFoo->emitFunction(val);        //重点, 静态类的实例发信号
    }
    static Foo *theFoo;                  // 重点，建立静态类的指针

28. double to string (Qt方法) ---另见c方法
    QString::number(doubleVal, 10, 小数精度);

28. double to string  (数字转字符串方法) ---另见Qt方法
    #include <sstream>
    ostringstream os;
    os << 2.12;
    string s = os.str();

28. hex string to hex number
    StrToHex (把16进制的可视字付串，表示为asccid码数字)

    #include <stdio.h>
    #include <stdlib.h>
    #include "math.h"
    #include <iostream> 

    unsigned long long StrToHex(const char *pstr)
    {
    unsigned long long ull = 0ULL;
    if (pstr != NULL) {
        while (*pstr != 0) {
            char ch = *pstr++;
            if (ch != ' ') {
                if (ch == '0' && (*pstr == 'x' || *pstr == 'X')) {
                    pstr++;
                    while (*pstr != 0) {
                        unsigned char uc = 0;
                        if (*pstr >= '0' && *pstr <= '9') {
                            uc = (unsigned char)(*pstr - '0');
                        } else if (*pstr >= 'a' && *pstr <= 'f') {
                            uc = (unsigned char)(*pstr - 'a' + 10);
                        } else if (*pstr >= 'A' && *pstr <= 'F') {
                            uc = (unsigned char)(*pstr - 'A' + 10);
                        } else {
                            break;
                        }
                        ull <<= 4;
                        ull |= uc;
                        pstr++;
                    }
                }
                break;
            }
        }
    }
    return ull;
}

////////////////////// 
int HexToStr(unsigned long long ull, char *pstr, size_t size)
{
    int ret = -1;
    if (pstr != NULL && size > 0) {
        char pref[] = { '0', 'x' };
        size_t cch = sizeof(pref);
        unsigned long long _ull = ull;
 
        while (_ull > 0) {
            _ull >>= 4;
            cch++;
        }
 
        if (size >= cch) {
            *pstr++ = pref[0];
            *pstr++ = pref[1];
 
            while (cch-- > sizeof(pref)) {
                pstr++;
            }
 
            *pstr-- = 0;
 
            while (ull > 0ULL) {
                unsigned char uc = ull & 0xF;
                *pstr-- = uc + ((uc < 10) ? '0' : ('A' - 10));
                ull >>= 4;
            }
 
            ret = 0;
        }
    }
    return ret;
}

int main(int argc, char *argv[])
{
    char str[32] = { 0 };
    char *hexstr = "0x00123abcdefgh";
    unsigned long long ullret = 0ULL;
    unsigned long long ulltest = 0x123456789ABCDEFULL;
 
    // 自己编写函数将字符串作为 16 进制数字字符串解释为整数
    ullret = StrToHex(hexstr);
    printf("StrToHex:\n%s -> 0x%llX\n", hexstr, ullret);
    // 怎么把16进制字符串转整型（无符号长整形）
    char mi = 0xfd;
    int base = 0x092c;
    float fnum= base * pow(10, mi-1);
    std::cout << ullret << std::endl;
    std::cout << fnum << std::endl;


 
    // 自己编写函数将整数格式化为以 16 进制表示的字符串
    if (HexToStr(ulltest, str, 32) != -1) {
        printf("HexToStr:\n0x%llX -> %s\n", ulltest, str);
    }
 
    putchar('\n');
 
    // 用 strtoull 将字符串作为 16 进制数字字符串解释为整数
    ullret = strtoull(hexstr, NULL, 16);
    printf("strtoull:\n%s -> 0x%llX\n", hexstr, ullret);
 
    // 用 sprintf/snprintf 将整数格式化为以 16 进制表示的字符串
    if (snprintf(str, 32, "0x%llX", ulltest) > 0) {
        printf("snprintf:\n0x%llX -> %s\n", ulltest, str);
    }
 
    putchar('\n');
 
    return 0;
}

27. 想办法
    1. 要把项目的文件生成一个张类图
    2. 把自已做的库及头文件也用make install 安装在系统指令位置。
        为了项目开发时使用方便
        (另外想法：指接引用到库的开发目录，便于边开发边使用)

26. Qt c++ 对应重复工作的预备
    1. 从已有项目中复制（main.cpp)复制类的实例化
    2. pro文件中添加该类(SOURCE +\ cpp; HEADER +\ H)
    3. cp这些文件到项目中
    ------
    4. 在新线程使用串口库 serial/serial.h

25. qt 快捷键

24. qt thread 较好的讲解
    << Qt thread: simple, complete and stable >>
    https://fabienpn.wordpress.com/2013/05/01/qt-thread-simple-and-stable-with-sources/

    << std::thread >>
    https://www.cnblogs.com/lidabo/p/7852033.html
    (　要用到 thread.join() 才不会出错)
23. 关于时差计算
        Qt计算时间的两种方法：

        QTime elapsed() : ms
        QTime currentTime() : ms
        C++计算时间的五种方法：

        clock() : ms
        GetTickCount() : ms
        gettimeofday(time_val*, NULL) : us
        QueryPerformanceFrequency(LARGE_INTEGER*) & QueryPerformanceCounter(LARGE_INTEGER*) : us
        time(NULL) : s

22. 测试与qt相关类 qtestlib
    https://www.cnblogs.com/qtgameprograming/p/4286778.html
    在QT 项目下的main.cpp改造一下
    测试的代码写在main.cpp中
    项目根目录下:
    qmake makefile  (生产命令行可执行的依赖)
    make 

21. 运行java命令行参数的方法：

void JavaThread::run(){
    //QProcess *pro = new QProcess();
    emit waitJava("系统准备中......");
    this->sleep(8);
     emit waitJava("系统准备就绪.");
    pro = new QProcess();
    QProcessEnvironment env;
    //env.insert("JAVA_HOME", "/usr/java/jdk1.8.0_201-amd64");
    env.insert("LD_LIBRARY_PATH", "/usr/java/jdk1.8.0_201-amd64/lib/amd64/");
    pro->setProcessEnvironment(env);
    QString cmd = "/usr/bin/java";
    QStringList args;
    args.append("-jar");
    args.append("/usr/bin/clj-pc-0.1.0-SNAPSHOT-standalone.jar");
    QThread *newThread = new QThread();
    pro->moveToThread(newThread);
    pro->start(cmd, args);
    pro->waitForFinished(-1);
    QByteArray bt = pro->readAllStandardError();
    // QByteArray bt = pro->readAllStandardOutput();
    QString str = bt;
    qDebug()<<str;
    qDebug()<< pro->state();

}
    
20. no
        void processmethodONE() {
         QThread* thread = new QThread;
         Prozess.moveToThread(thread);
         Prozess.start(ProcessComand);

19. qt 运行shell 本地命令
        QProcess process;  
        process.start("lshal -u computer -l");
        process.waitForFinished();    
        QByteArray output = process.readAllStandardOutput();
        QString str_output = output;
        qDebug()<<output;
        另外示例参见 bats项目：camera.cpp pro->start(cmd,args);

18. 为什么qt release程序
    单独打开，默认线程没有开启？还是数据库连接不上。
    那就在qt上加载一个messageBox提醒
    (可能要在目标机重新编译一次就ＯＫ了) 

17. qt 在其他用户看不到 (root 找不到qt应用)
    home/wmt/.config/QtProject/qtcreator  
    /home/wmt/Qt5.10.0/Tools/QtCreator/bin/qtcreator
    ./Tools/QtCreator/share/applications/org.qt-project.qtcreator.desktop

16. date time
   QDateTime now =QDateTime::currentDateTime();
   QString now_date =now.toString("yyyy-MM-dd");

15. QT表格居中显示 
        (1.tableview) 
        在data函数里面加 
        if(role == Qt::TextAlignmentRole ) 
        { 
        value = (Qt::AlignCenter); 
        return value; 
        } 

14. Dialog实例的打开
     Dialog *specA = new Dialog(this);
    specA->exec(); 

13. QMessageBox
    https://www.cnblogs.com/Peit/p/7493689.html
    QMessageBox::information(NULL, "Title", "Content", QMessageBox::Yes | QMessageBox::No, QMessageBox::Yes);
12. 多线程
    https://blog.csdn.net/naibozhuan3744/article/details/81174681
    没有提到创建信号的方法
    1[3~]. 新建一个类MyThread:QThread  
    2. MyThread 中的函数是信号，触发ＵＩ线程的函数（slot)
    3. myThread 可以定义线程sleep mssleep ussleep(秒/毫秒/微秒的延时)

11. cannot find -lGL  -> 需要安装openGL
    yum install mesa-libGL-devel mesa-libGLU-devel //这两个是opengl核心库
    yum install freeglut-devel //OpenGL Utility ToolKi
    (其它)
    sudo yum install gcc gcc-g++ cmake

10. qt  下载镱像
    5.10 百度一下　官方下载即可

9. 如何通过QThread自定义子线程来控制QT窗口控件
    https://blog.csdn.net/shiinayukari/article/details/77679593
        1.添加一个自定义类，继承QThread类，在类中添加MainWindow类型的指针MainWindow *w
        定义信号和槽函数，线程运行时发出信号，槽函数中调用MainWindow类内的控件操作函数。

        2.在MainWindow类中定义用于操作窗口内控件的函数，此处以操作ProgressBar为例

        **为什么要通过槽函数来操作窗口控件，而不是直接调用MainWindow的成员函数来操作控件？
       在新定义的线程中调用其他线程的窗口刷新功能，会导致系统提示不安全而报错。

8. 建立一个线程
    myThread *thread1 = new myThread;
    thread1.start();
        分析这个start函数在类里面是怎么写的？
        类的头文件中没有start这个函数，有run().  应该是其类有start()-> 调用run()的新实现


7. debug 打印出调试信息 (类似println)
    qDebug()<<"bisOpenn="<<bisOpenn;                    

6. 完成qt 操作psql
        find . -name sqldrivers
        ./Tools/QtCreator/lib/Qt/plugins/sqldrivers
        ./5.10.0/gcc_64/plugins/sqldrivers
        ./5.10.0/Src/qtbase/plugins/sqldrivers
        ./5.10.0/Src/qtbase/src/plugins/sqldriver 
    有一段样例程序:  https://forum.qt.io/topic/61595/how-to-connect-qt-with-postgresql
    第二个例子连接成功: https://bbs.csdn.net/topics/391819369
   
    《学习笔记》
    https://blog.csdn.net/ljt350740378/article/details/70237023
    首先要在pro文件中：　qt　+= sql
    cpp文件中：include <QTsql>
        QSqlDatabase类实现了数据库连接的操作
        QSqlQuery类执行SQL语句
        QSqlRecord类封装数据库所有记录
        QSqlDatabase类
    例：
     QSqlDatabase db = QSqlDatabase::addDatabase("QMYSQL", "localhost@3306");
    db.setHostName("localhost");    //数据库主机名
    db.setDatabaseName("databasex");    //数据库名
    db.setUserName("root");        //数据库用户名
    db.setPassword("123456");        //数据库密码
    bool bisOpenn = db.open();          //打开数据库连接
    qDebug()<<"bisOpenn="<<bisOpenn;
    //db.close();         //释放数据库连接



5. a.out 文件奇怪的运行方式
    LD_LIBRARY_PATH=/usr/java/jdk1.8.0_201-amd64/jre/lib/amd64/server ./a.out
    原来a.out要运行，它要找到动态库，前导的就是指定相关动态库的位置， 也可用export 变量表示

4. 调用clojure函数
    https://stackoverflow.com/questions/8650485/can-i-call-clojure-code-from-c
    JNIEvn *env // 本地方法接口的指针
    env->CallObjectMethod(load_string, load_string_invoke, env->NewStringUTF    ("(println 'wangmantao)"));
    先自我分析：
        接口调用对象方法（这里是立即执行方法的意思）。
            这个c方法原型:
        jobject (JNICALL *CallObjectMethod)
            (JNIEnv *env, jobject obj, jmethodID methodID, ...);
        jobject (JNICALL *CallObjectMethodV)
            (JNIEnv *env, jobject obj, jmethodID methodID, va_list args);
        jobject (JNICALL *CallObjectMethodA)
            (JNIEnv *env, jobject obj, jmethodID methodID, const jvalue * args);
        函数名释义：CallObjectMethod　　--- 返回值为obj的方法
        -------------------------
        第一参：env =... 本身实例吧
        第二参：obj = CallStaticObjectMethod (jclass, jmethodID, str, str )
        第三参：jmethodID = GetMethodID  (GetObjectClass(), str, str)
        第四参：args = str => "println ....."

    JNIEnv 的用法：
        使用JNI提供的反射借口来反射得到Java方法，进行调用。
        使用C语言调用jni的时候，需要和(java的环境对象_JNIENV)和(虚拟机对象_JavaVM)交互。

3. linuxdeployqt 就是不行
    (参考　　https://doc.qt.io/qt-5/linux-deployment.html)
    尝试静态build qt的库，放在temp/Qt5.10下，
        但是make了好久，暂时停工
    在做好的app目录下，重新产生Makefile,
        示例：　/home/wmt/Qt5.10.0/Examples/Qt-5.10.0/widgets/tools/plugandpaint
        make clean              (当前还没有编译过，不用作)
        PATH=/path/to/Qt/bin:$PATH  (刚编译的静态库有这bin目录？)
        export PATH
        qmake -config release       （qmake 命令是qt的静态bin?)
        make                        (看下新的Makefile 有提到新的静态目录吗)

2. 程序打包  https://blog.csdn.net/zjx18915341085/article/details/79715075
[patchelf(必须)]()
[linuxdeployqt](https://github.com/probonopd/linuxdeployqt)

linuxdeployqt-continuous-x86_64.AppImage untitled -appimage


1. qt 设置程序图标
    https://www.iconfinder.com/search/?q=program (icon下载)

