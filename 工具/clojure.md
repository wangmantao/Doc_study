# clojure 用法study [w3c](https://www.w3cschool.cn/clojure/)
[timer](https://bjeanes.com/2012/09/call-clojure-function-on-a-timer)
以前做项目参考：/home/wmt/myProj/王满涛/clj-serial

## 学习灵感 (完成后备份到分割线以下)
29. serial-port 实验，得到已存在的port之path值
    用它再打开port,　并且再onByte
    (portids [] ) 返回一些port的标识符
    (port-at [th]) 返回第几个port的Name

28. 数学库 math
    (Math/abs -1) 绝对值

27. port数不对时，把/run/lock/L* 文件删掉，再找port数
26. 
什么是clojure的副作用(side effect)： 

        纯函数（Pure Function）：输入输出数据流全是显式（Explicit）的。函数与外界交换数据只有参数和返回值。
        隐式（Implicit）函数：函数可以通过参数和返回值以外的方式和外界交换数据。eg:修改全局变量,利用I/O读取/输出到文件文件,打印到屏幕等等

        借用一个例子,比如open是打开文件的函数,它打开一个文件,然后你可以对文件进行操作，比如open('C:/test.txt'),这个语句的返回值是1或者-1(文件是否存在)，这是这个函数的作用，而它的副作用就是打开了一个文件

25. Software Transactional Memory (STM) 
        (def droid (ref (struct item "Droid X" 0)))
        (def history (ref ()))
        两个ref变量要事务中同时被改变，保持了数据的完整性！

        如果一个线程利用数量 25 来调用 place-offer 函数，
        同时，另一个线程利用数量 22 调用同一函数，将会发生什么情况？
        Clojure 会确保在事务中间该值没有发生改变，因此，如果事务到达了 dosync 块的结尾，并且，STM 可看到在当前事务启动以来有另一事务已经完成了，那么当前事务将回滚并再次运行。
        这使得，仅纯函数 —— 也就是不具有副作用的函数 —— 才能作为事务的一部分，因为函数可能将被运行多次。Clojure 采用性能非常高的永久数据结构，来确保此类事务/回滚的效率。

24. 事务与引用
        ava.lang.IllegalStateException: No transaction running
        您不能在不首先启动事务的情况下更改引用类型的值。
        alter命令放置在dosync
        我们有两个值在dosync块中被改变。 如果事务成功，则两个值都将改变，否则整个事务将失败。

23. 由“事务”引起的搜索
    (https://www.w3cschool.cn/clojure/clojure_concurrent_programming.html)
    涉及多个处理器的共享数据时，有必要确保在使用多个处理器时保持数据的状态的完整性。 这被称为并发编程，Clojure提供对这种编程的支持。
   通过dosync，ref，set，alter等暴露的软件事务存储器系统（STM）支持以同步和协调的方式共享线程之间的变化状态。  
    事务

        Clojure中的并发是基于事务。 引用只能在事务中更改。 在事务中应用以下规则。
        所有的变化都是atomic和孤立的。
        对引用的每个更改都发生在事务中。
        没有事务看到另一个事务所造成的影响。
        所有事务都放在dosync块中。
        我们已经看到了dosync块做了什么，让我们再看看。
        dosync

        在包含表达式和任何嵌套调用的事务中运行表达式（在隐式do中）。 如果此线程上没有运行，则启动事务。 任何未捕获的异常将中止事务并流出dosync。
        以下是 dosync 的基本语法。

22. Ref
    (https://blog.csdn.net/shann09/article/details/51700281)
    Ref是类似Atom和Agent的变量，包含一个所有线程共享的值，Ref只能在STM事务中修改，能够修改的函数有：ref-set，alter，commute，获取Ref值的方式是用@读取器或者deref，读取的话不需要在事务内部，但是要读取多个ref的一致性快照的话就需要在事务内部。
    alter和commute的第一个参数是一个Ref，第二个参数是一个能够返回新值的函数，简称参数函数，当参数函数被调用的时候，它会获得如下参数：当前Ref的值、alter或commute接收到的其它参数，参数函数返回的值作为Ref的新值。


21. dosync (clojure.core)
     这是一个宏，他的作用是将表达式包装在一个事务里，如果当前线程没有事务，那么就启动一个。

20.   go  (clojure.core.async)
    Asynchronously executes the body, 
    returning immediately to the calling thread. Additionally, 
    any visible calls to <!, >! and alt!/alts! channel operations within the body 
        will block (if necessary) by 'parking' the calling thread rather than tying up an OS thread.
        通过“打包”子线程，要发于捆线主线程（OS Thread).
        在函数体内部通过一些通道操作functions,(如alt!)可以阻塞线程。
    Upon completion of the operation, the body will be resumed.
    只到通道操作完成，go内部的代码恢复执行。
     Returns a channel which will receive the result of the body when completed
    go内部代码执行完毕的结果，返回的channel接收这个结果。

19. 线程id (thread id)
 (.getId (Thread/currentThread))

18. 运行命令行　command line shell
    (use '[clojure.java.shell :only [sh]])
    (sh "free")
    (sh "top" "-bn1")

    有效:
    (sh "sh" "-c" "cpuid -1 |grep serial")
    {:exit 0, :out "      PSN: processor serial number           = false\n   processor serial number: 0003-06A9-0000-0000-0000-0000\n", :err ""}
    匹配出结果来。。。 
    (re-find #"\d*" a) 
    [^\s]{4}-[^\s]{4}-[^\s]{4}-[^\s]{4}-[^\s]{4}

17. 程序加密 混淆 obfusticator
    cpuid
    processor serial number: 0003-06A9-0000-0000-0000-0000
    先了解字节码反编译的工具：(有网络再下载)
        https://github.com/Konloch/bytecode-viewer/releases

     再看反编译工具：
        Pro Guard (用法　https://www.owasp.org/index.php/Bytecode_obfuscation)
           yGuard
    

16. 获得当前时间（ms) (long型)
    System/currentTimeMillis

15. 间隔调用（定时器 timer)
        (defn set-interval [callback ms]
          (future (while true (do (Thread/sleep ms) (callback)))))
    
        -- 分析 在新线程中一直执行 callback/ms , 用while给了一个条件开关.
        -- 用法　给它一个变量，让它一直累加这个变量

        (def job (set-interval #(println "hello") 1000))
         =>hello
           hello
           ...

        (future-cancel job)
         =>true
        -- 分析 set-interval 返回的是一个future, 可以用future-cancel来终止
                (但future不能用time来返回它运行时间，因为future 返回的是它内部的表达式返回值)

14. doseq break (像while 退出循环)
    不能这样做，改用every? not-every?
    有一些谓词函数测试集合里面每一个元素然后返回一个布尔值，这些函数都是”short-circuit”的，
    一旦__它们的返回值能确定它们就不再继续测试剩下的元素了__，有点像java的&&和or, 比如:
        (every? #(instance? String %) stooges) ; -> true
        (not-every? #(instance? String %) stooges) ; -> false
        (some #(instance? Number %) stooges) ; -> nil
        (not-any? #(instance? Number %) stooges) ; -> true    

        ser=> (defn ck [n] (if (> n 10) true false))
        user=> (every? ck [1 2 3 4])
                                        false
        user=> (every? ck [11 12 13 14])
                                        true

13. 统计一个函数花费的时间 timer    
    time(Thread/sleep 1000)
    由简单到高级的库　https://stackoverflow.com/questions/21404130/periodically-calling-a-function-in-clojure

12. 用一个序列产生另一个序列
    用map　filter 等函数，用另一个函数作为产生新序列的工具
    map就是把处理的结果组织起来，也让老序列的每个元素依次交给工具处理

11. 对一个序列(seq)，或是向量(vector) 求最大，最小
    (max [ 1 3 3 ])     
    (apply max seqVar)  apply是调用函数max, 把序列参数依次调入

10. 求绝对值
    #(Math/abs %)

9. 科学计数法（指数) -> 双精度　浮点数
       (Double/parseDouble "str")   

28. (= str str) 有问题
    对策－> 转换16进制再比较
    (defn hexify [s]
     (format "%x" (new java.math.BigInteger (.getBytes s)))

27. CallVoidMethod from clojure
     示例是在代码中嵌入了clojure代码, 但是这个嵌入代码应该没有methodId的,它是怎么指定id的，

     关键是这个代码我看不懂

        6.2 访问字段和方法
        JNI允许本地代码访问字段并调用Java对象的方法。
        JNI通过它们的_符号名_和_类型签名_-->标识方法和字段。
        两个步骤从字段或方法的名称和签名中, 提取出定位该字段或方法。
        例如，要调用_cls类_中的_方法f_，本地代码首先获得一个方法ID，如下所示：
            jmethodID mid = env->GetMethodID(cls, “f”, “(ILjava/lang/String;)D”);
                                             类,  方法的符号, 签名
        然后，本地代码可以重复使用方法ID，如下所示:
            jdouble result = env->CallDoubleMethod(obj, mid, 10, str);
                                                 对象，方法id, 参数
        字段或方法ID不阻止VM卸载派生该ID的类。卸载类后，方法或字段ID无效。因此，本地代码必须确保:
                                            (类还可以卸载！)
        如果它打算在较长时间内使用方法或字段ID,
        保持对底层类的实时引用，或重新计算方法或字段ID

        JNI没有对内部如何实现字段和方法ID施加任何限制。


26.
        Warning: specified :main without including it in :aot. 
        Implicit AOT of :main will be removed in Leiningen 3.0.0. 
        If you only need AOT for your uberjar, consider adding :aot :all into your
        :uberjar profile instead.

        解决方法:  in project.clj
        :profiles {:uberjar {:aot :all}}


25. load-string
    (把string 反射成命令语句i)
    
24. c++ 调用clojure (另见st qt)
    https://stackoverflow.com/questions/8650485/can-i-call-clojure-code-from-c
    https://stackoverflow.com/questions/40322749/calling-a-clojure-function-from-haskell (Calling a Clojure Function from Haskell)

    关键: jni.h
    输出的.out文件不能直接运行(以下解决办法不行)，编译的过程已出现运行结果

    [build中的文件]
    java-1.8.0-openjdk-1.8.0.171-3.b10.fc26.x86_64

    修改1:
    ln -s  ..._201... /usr/java/latest
    /etc/alternatives/java > /usr/java/jdk1.8.0_201-amd64/bin/java  (还有javac)
    JAVA_HOME /usr/java/jdk1.8.0_201-amd64/ 



23. 竟然还有clojurec for c 的编译器
    https://github.com/schani/clojurec

22. 为什么要生成class   
    (gen-class)
    无论程序入口，或空间类调用，都须要找类名

21. java -jar clojure项目.jar

20.  Clojure语法关键字数字支持Java的数字类型浮点数后面用后缀M表示
    Clojure 提供了可以支持无限精度的 “大数” 字面量。你只需要在整数字面量后面加上大写的 N，就可以把它变为可支持 无限范围[2] 的 “大整数”。顺带一提，如果在小数后加上大写的 M，就可以把它变为无限精度的小数。大数类型和普通类型进行运算，结果自动变成大数类型

19. system property(没用)
	system.setproperty（“gnu.io.rxtx.serialports”，“/dev/ttyacm0”）	
	System/setproperty "gnu.io.rxtx.serialports" "/dev/ttyACM0)	

18. str 与 conj
	一个是连接成字符串，一个是连接成list

17. 串口数据的获取与处理
	建立一个byte_handle来处理（当前的字节）对最后的结束字符进行处理
	
16. serial-port 怎么样才能完成读
	最简单的读字节的方式，是on-byte. 它可以让你注册一个fn函数，每个字节来时，函数都会被调用。
	on-byte port #(println %))
	也可以做成每n个byte一个事件,比如两个字节对，一个表示按钮的状态，另个字节表示按钮的位置
	(on-n-bytes port 2 (fn [[action coords]] ...))
	如果你想想得到从InputStream的元数据，也可以用函数listen. 这个可让你指定一个handler被调用，每次有一个数据在port,　你可以直接处理input stream.
	当这个handler直接被处理，已初缓冲的字节会被丢掉，但是这个方式可以通过on-byte的第三个参数为falseg来改变。
	一次只能注册一个listener,　如果你想分开输入的数据流，到另个串流，你可以考虑使用lamina.
	最后要 remove-listener.
	
15. 获得集合的长度 
	count 集合
14. def x (byte-array [(byte 0x43) (byte 0x6c)]  创建的byte数组，(String. x)

13. &:表示任意数量的可选参数。
 	fn [a b &c]  

12. import 是导入java空间中的各个类的
	(:import [空间1 类１类２]  [空间2 类1 类2])

11. ^表示什么意思 (^ 后跟java类型)

10. defrecord 用于创建java类(实例)
	
9. 串口的用法
	加载依赖库
	[com.github.purejavacomm/purejavacomm "1.0.2.RELEASE"]
	项目core.clj中定义了几个接口,和串口参数宏定义
	(其实这个core.就是一个使用purejavacomm的串口用法示例)

	defrecord Port 
	defn- raw-port-ids
	defn port-identifiers
	(def ^{:deprecated "2.0.3" :doc "Deprecated; use `port-identifiers`"}
 	port-ids port-identifiers)
	port-identifier
	close!
	(def ^{:deprecated "2.0.3" :doc "Deprecated; use `close!` instead"}
 		close close!)
	open
	(defprotocol Bytable
	(extend-protocol Bytable 
	(defn- write-bytes
	write
	skip-input!
	listen!
	(def ^{:deprecated "2.0.3" :doc "Deprecated; use `listen!` instead"}
150   listen listen!)
	unlisten!
	(def ^{:deprecated "2.0.3" :doc "Deprecated; use `unlisten!` instead"}
 	 remove-listener unlisten!)

	
8. lein run 可以；uberjar 后却不能运行
	uberjar时的警告：
	arning: specified :main without including it in :aot.  
	Implicit AOT of :main will be removed in Leiningen 3.0.0.           
	If you only need AOT for your uberjar, consider adding :aot :all into your                                      
	:uberjar profile instead.   
	Compiling io.ryos.tars.mycmds  
	----> 用了:aot :all 后，找不到io/moo/tars/dsl.clj classpath

7. Daemon移植到其它电脑
	要重新从官网下载jre环境（rpm安装）
	不能找到daemon loader org/apache/commons/daemon/support/DaemonLoader
	--> 对策方法：
		run.sh (jsvc启动脚本)	
		1. 加上参数 -cwd /当前/工作/目录
		2. -cp commons.daemon.1.x.x.jar:./daemon_api.jar
6. 籽了把clj-serial 也做成公用库
	upLocalRepo.sh 
	.产生的JAR名字，有project.clj定义的(所以项目目录名和项目名并不一直)
5. lein-jarbin (为了daemon; 不进行了)
	lien插件，运行包含jar的二进制文件
	用作复杂系统的二进制或者守护程序，没有安装步骤．

4. 把clojure 做成守护进程
	<< Clojure 经典实例 >> 8.4  (P. 321)
	用到Apache Commons Deamon库
	1. 应用必须实现接口：Deamon (org.apache.commons.deamon.Deamon)
	2. 做一个系统应用(unix: jsvc / windows: procrun)

- project.clj 
	:aot :all ---> 什么意思	
	:dependencise [org.apache.commons/commons-deamon "1.0.9"]
	:main　空间.类 (没有引号，没有扩展名)(即:项目名.core)
	
- ns申明时:
	1. :import
	2. :gen-class ->实现基于 :implements

- 定义变量时：
	变量的值设为atom类型
	(def last_byte (atom 0))

- 定义函数时：
	1. init [args] / start [] / stop []
	2. deamon的实现
	   -init[this ^DeamonContext context] -> 应用初始化
	   -start[this] ->　开启新线程执行工作　-> jsvc接手．
	   -stop[this]  -> 要停上start 启动的线程
	   -destroy[this] -> 可以写一具空函数给jsvc看
	3. 命令行调用 -main [& args]  (做冒烟测试，接口不要动)

- 用jscv 执行
	java_home ..................................

3. 把jar制作成守护进程
	
2. 线程交互
	利用了watch功能．当引用类型变量（atom)值改为时，atom的监视函数会被调用．　参考：　add-watch

	
## 知识点滴 (倒序)
24. jsvc 调用class类库（aot所编译）,
	入口函数在-start上，内用future其它线程调用

23. java_home 问题
	which java -> /usr/bin/java -> /etc/alternatives/java ->  /usr/java/jre1.8.0_151/bin/java

22. jsvc / procrun  (java service / process run)
	JVM 与操作系统的中间层，Ｃ编写;创建一个后台执行java应用

21. :import java类的空间名 类１名　类２名　（import的格式）

20. 实例方法（参数是this)

19. 函数连接号"-main" 告诉clojure编辑器，它是一个JAVA函数

18. cron任务调用
	linux下的定时任务, 由crontab设定

17. filter
	(filter fn map)
	把map的每个元素逐个给fn进行处理辨别，如果为真这个元素被筛选出.
	例：　(filter #(= std_val %) map_array)

16. %符号
	% 必须与#() 内部用
	可能是代表一个自动形成的参数（是自动变化的)
	用在匿名函数中(#), 表示它的一个参数，这个参数从哪来呢?

16. distinct
	意思：有区别的. 作用：在一组数中只筛选出有区别的数

15. (:key map)
	直接获得map对应key的val

14. clojure.java.shell/sh 
	调用命令行

13. add-watch 
	(add-watch reference key fn)
	fn 必须有四个参数(keyId ref oldVal newVal)

12. assoc 
	(assoc map key val)(assoc map key val & kvs)	
	往已经map中添加指定的:key val对;如果key已经存在，则更新val;
 	可以写多份:key val :key2 val :key3 val	

11. update-in 
	(update-in m ks f & args)
	详：(update-in map keys fun & args)
	keys是多元素的向量，所以写法是：［:key1]
	f处是函数　(str " somestr") -> 更新为: "原内容　somestr"
	更新嵌套map结构中指定key的内容，用函数改变旧指返回新值.

10. 用swap!改变atom值的方法
	(swap! atom f)(swap! atom f x)(swap! atom f x y)(swap! atom f x y & args)
	f: conj; inc; dec; update-in; identify; assoc;  str; codj
	x: f的第一个参数　y: f的第二个参数
	说明，swap! 函数的第一个参数是atom变量，第二个参数是一个函数及其参数。（中间无括号）

9. 加载依赖 jdbc
	project.clj :dependencies [[org.coljure/java.jdbc "0.6.1"]
				  [org.postgresql/postgresql "9.4.1211"]]
	类.clj :require [clojure.java.jdbc :as jdbc])
    (jdbc/update! pg-db :logs {:end_time end_time}["sn = ?" sn])
    (jdbc/insert! pg-db :training {:user_sn (get-user-sn name) :word_sn (get-word-sn word)

    
8. java对像/实例的方法调用 (前面加点)
	(.getTime Date类) (.toString javaObject)

7. project.clj :main 的写法
	:main 含有main函数的空间名 (空间名无需引号)
	main函数的定义 (def -main [ & args ])

6. lein-localrepo 本地库用法 (opencv clojure 安装所讲)
[github](https://github.com/kumarshantanu/lein-localrepo)
(在profiles.clj中配置它为全局插件)
	__localrepo用法:__
	lein localrepo install [-r repo-path] [-p pom-file] <filename> <[groupId/]artifactId> <version>
	例：lein localrepo install foo-1.0.6.jar com.example/foo 1.0.6

	lein localrepo list [-r repo-path] [-s | -f | -d]
	lein localrepo remove <[groupId/]artifactId> [<version>]
	例：lein localrepo remove com.example/foo 

5. clojure 项目配置文件 ~/.lein/profiles.clj 

4. sting->number转换 (if number? (read-string "2"))
	read-string 属于 clojure.tools.reader <见github> 

1. *string <=> date*
　　（.parse <=> .format) 
例：
(.format
    (java.text.SimpleDateFormat. "dd.MM.yyyy") ;; 参数１，要字符串化的样子
    (.parse  (java.text.SimpleDateFormat. "ddMMyyyy") "08082013")) ;; 参数２，一个jave.util.Date 类型

(.parse 参数１待指定字符串的格式　参数２符合前面格式的字符串)

2. *getTime ---获得当前时间整形*
java语言 Date类的方法 
(.getTime (java.util.Date.))  ;;Date实例的成员函数，获得当前时间(类型java.lang.Long 整形)

3. *timer制作方法*
- (future  & body): body部分被另一个thread调用; 可以(def f (future ...)) @f--同步(阻塞式)取返回结果
- Thread/sleep int: 定时器延时必有项
- doseq [one somes]: 取序列的每个项并做处理
- repeatedly n fn:  结合一个可有副作用的函数(无参数)，该函数被重复(无限次或n次)调用，每次调用返回结果组成Lazy seq(懒惰序列)．
- apply fn args: 类似reduce + [1 2 3] 第个参数迭加运算；而它是把每个参数依次带到函数fn运行．

```
(defn tick
  "Call f with args every ms. First call will be after ms"
  "每x毫秒调用一次f函数（带参数），首次调用会有x毫秒的延迟"
  [ms f & args]

  (future  ;; 以下body用新线程执行
    (doseq [f (repeatedly #(apply f args))]  ;每个seq子项依次执行，
      (Thread/sleep ms)
      (f))))
```
简单的做法：　loop f recur 缺点是无限循环；不可控；无参数

## java.text　空间(包)
[SimpleDateFormat类](https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html) (java.text.SimpleDateFormat.)

## java.util　空间(包)

---------------------------------------------------------------------------------------------
## 学习灵感 (备份)

### 建自已的clojure库
1. coljure 怎么建自己的类库util.clj，每个项目可以自动被加载，类似本地的maven reps.
(当前就为了完成建一个库，内有根据人员调出ＩＤ号的函数, 并把API做的正规/文档化一点)

2.  建立一个项目(cljutil)，定义一个空间ns, 其内定义一个函数（分公用和私用）做一个最简demo
	ns: cljutil.core/foo
	lein uberjar  生产一个jar, 确定它的位置: targetSNAPSHOT-standalone.jar

3. 把jar它做成本地mvn repostory
	> lein localrepo install /home/wmt/temp/test/cljutil/target/cljutil-0.1.0-SNAPSHOT.jar cc.taily/cljutil 0.1.0
	> lein localrepo list -f |grep cljutil
	[cc.taily/cljutil "0.1.0"]     cljutil-0.1.0.jar                   7250 2018-11-13 23:24:20

4. 新建另一个项目utiltest，让它调用以上　jar 工作包 -> 查看调用运行结果
	(project.clj 中: 	:dependencies: [[其它][groupId/artifactId "version"]] (需求库)
	(在使用的源文件中：　	:require [cljutil.core] as xx)
	(cljutil库函数应用方法为：　（cljutil.core/foo "wmt")    )

5. 变更cljutil函数内容，再次lein uberjar, 再次MVN local reporstory
	(重复执行以下内容－－－将来可用脚本执行, 或lein uberjar 后就直接自动执行脚本)
	lein uberjar
	lein localrepo install /home/wmt/temp/test/cljutil/target/cljutil-0.1.0-SNAPSHOT.jar cc.taily/cljutil 0.1.0

6. 查看utiltest运行结果是否有变.
	(变更成功)

7. 确定cc.taily.util这个项目源码建在哪里，本地化的脚本写在哪里，怎么运行，有哪些功能函数．


