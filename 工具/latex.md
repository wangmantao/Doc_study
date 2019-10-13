# Latex 学习笔记

## 灵感

6. vim 插件技巧
	:TTemplate -> 选择模板
	<F5> 	  -> 插入环境　（eviroment)
	c+j       -> 插入模式下跳入该填的空
5. vim 插件
	https://github.com/vim-latex/vim-latex
-	[help](http://vim-latex.sourceforge.net/documentation/latex-suite-quickstart.html)
4. 制作一个表格，统计知识的组织模式，以便挖掘
	文件放在：  NORMAL  t.tex
	/home/wmt/myProj/未明确/tex/tab/t.tex

3. pgf包的问题解决(gpfmanual.pdf)
	可以pgf_3.0.1.tds.zip 包内的tex目录放入~/texmf(无则建)
	pdf编译时，则会从以上目录调用．
	其时texLive2018有PGF包，但不是知是否最新
	texLive2018 自带： (/usr/local/texlive/2018/texmf-dist/tex/latex/pgf/
	新安装: /home/wmt/texmf/tex/
	
2. texLive的结构：
-	[安装](https://blog.csdn.net/luoluo19550418/article/details/80503778)
	主目录: /usr/local/texlive/
	bin: /usr/local/texlive/2018/bin/x86_64-linux

1. 用Latex 的Tikz包
	找个demo
	_PGF and TikZ_: 
	PGF: 是Tex宏包，用来生成图形
	Tikz: 是用户友好的语法layer.
	下载：pgf_3.0.1.tds.zip 下载至/user/local/texlive/...
-        [环境安装与配置](http://www.latexstudio.net/archives/9774.html)	
	rebuild TEX’s filename database. This is done by
running the command texhash or mktexlsr

## 知识点滴
11. enumrate item 设置它们之间的行距 --- setlength
 \setlength{\itemsep}{0.1ex} %条目间距
  \setlength{\itemindent}{2em} %标签缩进量

10. 粗横线 hline -> Xhline {1.0pt}    usepackge {makecell}
9. 下划线 \underline{\hbox to 1.5cm{}}
8. 段落可以空两行来实现
7. 元素位置依次排列 node[shift={(30:7pt)}]  [above right]
6. 不要页眉页脚，页码	\thispagestyle
5. 罗列流程图的文字元素
4. 插入一页 	\clearpage (or \newpage)
3. 居中标题	定义\title{xxx} 后在适当的位置\maketile ->副作用：所在页格式为plain
2. A4 		\usepackage[a4paper, left = 2cm, right = 2cm]{geometry}	 
1. 中文支持 	\documentclass[11pt]{ctexart} -> xelatex编译（不是pdflatex)
	

## 资源
[Latex--TikZ和PGF--高级文本绘图，思维绘图，想到--得到！](http://www.cnblogs.com/tsingke/p/6649800.html)
[sourceForge.net有样例和源码](https://sourceforge.net/projects/pgf/)
