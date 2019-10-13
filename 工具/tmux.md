# tmux 用法笔记

神来之笔
## 用法
7. 窗口最大化
	ctr-b z (最大化panle)

1. 新建会话
在tmux外：	tmux new  -s <name-of-my-session>
在tmux内：Ctrl-b :   -> 输入命令 new -s <name-of-new-session>　

2. 会话change
Ctrl-b s

3. tmux 配置成Vi模式, 但进制制模式有效
~/.tmux.conf

4. 进复制模式有效
ctrl-b [  空格开始选 enter确认
ctrl-b ] 粘贴

5. 改变window大小
ctrl-b ctrl-箭头

6. 会话 
C-x s 以菜单的方式查看并选择会话
C-x :new-session 新建一个会话
C-x d 退出并保存会话
