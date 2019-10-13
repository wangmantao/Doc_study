# opencv 用法学习

## 学习灵感


## 知识点滴

###  用于clojure 的opencv库的安装

4. 获得imge(可以转换成jpg图像)
        IplImage* frame, newJpg;
       1. frame = cvQueryFrame(capture);
       2. frame = cvLoadImage(file_img, 1) 
       cvSaveImage(file_dst, jpg);
        问题是，IplImage 能否被ssocr识别     
    
3. qmake中使用opencv 
     项目名.pro中添加:
     CONFIG += link_pkgconfig
     PKGCONFIG += opencv


2. 查看opencv 的位置 (强大的pkg-config)
     pkg-config --modversion opencv
     pkg-config --cflags opencv
     pkg-config --libs opencv
    !!!连用
    pkg-config --cflags --libs opencv`

Install the _localrepo_ **Leiningen plugin**
lei-localrepo是jar库做为本地的repostory的插件
maven repostory　在本地的位置 ./.m2/repostory
这样就可以使clojure/java使用本地的opencv库了

1. 要使任何clj项目可用的库包（通过java接口）
home /.lein/profiles.clj
内容：
{:user {
	:plugins [
			[lein-localrepo "0.5.2"]
		]}}
当前:user下的任何新lein项目自动下载该库

2. 把某个java库，作为本地的repository
找到opencv的java库：
自建/opt/opencv/build/目录中，
- 有opencv-247.jar
- 有libopencv_java247.dylib (或.so文件）

3. 1. 把需要的库(jar),放到某位置
　　cp opencv-247.jar /opt/opencv-lib
3. 2. 把需要的库(so),放到某位置
   1, 要把so文件转成　jar文件
  　 用jar命令，从一个目录创建一个新的jar文件
  　　在opencv-lib中运行: 
  　　jar -cMf opencv-native-247.jar native
     这个就把so文件（类库文件）转成了jar调用文件了
4. 产生的两个库,用lein localrepos install 到 maven repos中
5. lein deps 在项目中运行（此时project.clj 已经写入了上面两个库的依赖
6.  repl调用　(clojure.lang.RT/loadLibrary org.opencv.core.Core/NATIVE_LIBRARY_NAME)
      出错： ClassNotFoundException: org.opencv.core.Core
      方法：看下jar中是不是这个类名结构

7. 遇坑问题
以上两个就是jvm与Opencv交互所需要的库.
 (如果没有以上java相关的库，则是因为本系统没有java_home系统变量,并且不要用root编译)
 (在opencv源码目录中，用cmake查看　opencv modules: 含不含java)

8. 没有jar库的解决办法
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -DBUILD_TESTS=OFF .. 
仍不行:
Found apache ant 1.10.1: /bin/ant
-- Could NOT find JNI (missing: JAVA_AWT_LIBRARY JAVA_JVM_LIBRARY JAVA_INCLUDE_PATH JAVA_INCLUDE_PATH2 JAVA_AWT_INCLUDE_PATH)

8. 最终结果
不能在root下编译, JAVA_HOME的变理好像找不到（或在root下不能生效)
