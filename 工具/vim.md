# vim用法

# 灵感
5. gvim -v
    -v			Vi 模式 (同 "vi")
   -e			Ex 模式 (同 "ex")
   -E			Improved Ex mode
    -v 模式下可以用来 "+y 复制到clipboard

4. vim-latex (ll 编译　lv 看PDF)

3. vim　插件 (可能很有用，有时间研究)
	Plug 'SirVer/ultisnips' 
	Plug 'honza/vim-snippets'
-	[教程](https://www.jianshu.com/p/720b369dd583)

1. emacs的配置文件.el
	emacs 使用 auto-complete clang 	
	vim 有对应的插件吗？

## 知识点滴
2. vim-plug 插件管理工具(不用bundle)

1. 窗口操作
改变窗口 高度: CTR-"="(即为"+") CTR-"-"
改变窗口 宽度: CTR-"<" CTR-">"

## 资源 (以下没分好类)
[Vim 8 下 C/C++ 开发环境搭建](http://www.udpwork.com/item/16798.html)
(讲到了管理插件的插件 vim-plug, 有配置vim-fireplace)
Plug 'tpope/vim-fireplace', { 'for': 'clojure' }
这是配置文件的一行

先把该工具vim-plug.vim放入.vim/autoload/目录下
而用vim-plug只要 ３种：
1. call plug#begin('~/.vim/plugged')
2. Plug '...'
3. call plug#end()

## 用到的其它插件
### 好看的状态条
[vim-airline](https://github.com/vim-airline/vim-airline)
技巧：
- 自动截断
- 智能标签条(顶端显示buffers)
　　default它是关闭的，开启：let g:airline#extensions#tabline#enabled = 1
　　buffer的标签间隔符可以改变：
　　　let g:airline#extensions#tabline#left_sep = ' '
 　　let g:airline#extensions#tabline#left_alt_sep = '|'

### Clojure for Vim IDE
[fireplace.vim](https://github.com/tpope/vim-fireplace)

__学习资料__
　　[Clojure with Vim and fireplace.vim](http://clojure-doc.org/articles/tutorials/vim_fireplace.html)
:help fireplace [Doc online](https://github.com/tpope/vim-fireplace/blob/master/doc/fireplace.txt)
　　[nREPL docs](https://github.com/clojure/tools.nrepl)

nREPL:__network REPL__ (提供了服务端和客户端)
(利用一些API,完成远程求值环境)

fireplace.vim (自动搞清楚端口号) __nREPL port__ 在 clj项目目录的target/repl-port文件中。 lein repl时自动生成的.

测试用clj文件
(deftest pairs-of-values
   (let [args ["--server" "localhost"
               "--port" "8080"
               "--environment" "production"]]
      (is (= {:server "localhost"
              :port "8080"
              :environment "production"}
              (parse-args args))))) ;; 运行参数解析

运行测试 fireplace 第一个命令: cpr -- (意为：Clojure Require|RunTest)
（这种命令没有冒号，直接在normal下瞎按)
