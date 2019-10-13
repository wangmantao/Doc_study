# Callgraph
[参考１](https://blog.csdn.net/guodeqiangde/article/details/78980733)
[参考2](https://blog.csdn.net/Solstice/article/details/488865)

## 灵感
1. 有cflow 生成函数调用关系


## 知识点滴
1. cflow
 cflow -b main.c -o ./cflow/函数调用关系x.txt __每个函数调用的子函数__
 cflow -x -b main.c -o ./cflow/函数调用关系x.txt __子函数们被哪些函数调用了__

2. callgraph
 callgraph -f main -b firefox __生成指定函数main调用的子函数集(svg图形)__

## 安装
### 三个工具组成
1. cflow (or calltree) 生成C函数调用树
2. graphviz (dot文本图形语言工具）
3. tree2dotx 把 C 函数调用树转换为 dot 格式的脚本

### 三个安装
1. sudo dnf install cflow
2. sudo yum install graphviz
3. tree2dotx & graphviz
$ wget -c https://github.com/tinyclub/linux-0.11-lab/raw/master/tools/tree2dotx
$ wget -c https://github.com/tinyclub/linux-0.11-lab/raw/master/tools/callgraph
$ sudo cp tree2dotx callgraph /usr/local/bin 
　　　　__tree2dotx__ 需要gawk, 可yum install gawk (查找替换文本工具)
$ sudo chmod +x /usr/local/bin/{tree2dotx,callgraph}


## 使用
1. 下载实验代码
　　git clone https://github.com/tinyclub/linux-0.11-lab.git && cd linux-0.11-lab
2. 
