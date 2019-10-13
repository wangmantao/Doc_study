# git版本控制的用法


## 资料方式
1. git help
    用法：git [--version] [--help] [-C <path>] [-c name=value]
               [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
               [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
               [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
               <command> [<args>]

    这些是各种场合常见的 Git 命令：

    开始一个工作区（参见：git help tutorial）
       clone      克隆一个仓库到一个新目录
       init       创建一个空的 Git 仓库或重新初始化一个已存在的仓库

    在当前变更上工作（参见：git help everyday）
       add        添加文件内容至索引
       mv         移动或重命名一个文件、目录或符号链接
       reset      重置当前 HEAD 到指定状态
       rm         从工作区和索引中删除文件

    检查历史和状态（参见：git help revisions）
       bisect     通过二分查找定位引入 bug 的提交
       grep       输出和模式匹配的行
       log        显示提交日志
       show       显示各种类型的对象
       status     显示工作区状态

    扩展、标记和调校您的历史记录
       branch     列出、创建或删除分支
       checkout   切换分支或恢复工作区文件
       commit     记录变更到仓库
       diff       显示提交之间、提交和工作区之间等的差异
       merge      合并两个或更多开发历史
       rebase     在另一个分支上重新应用提交
       tag        创建、列出、删除或校验一个 GPG 签名的标签对象

    协同（参见：git help workflows）
       fetch      从另外一个仓库下载对象和引用
       pull       获取并整合另外的仓库或一个本地分支
       push       更新远程引用和相关的对象

    命令 'git help -a' 和 'git help -g' 显示可用的子命令和一些概念帮助。
    查看 'git help <命令>' 或 'git help <概念>' 以获取给定子命令或概念的
    帮助。

## 提问答疑方式
3. 如何在本地，同步服务器上的最新的仓库
    git checkout (不是它)
    git fetch origin  
    git branch -a
    git rebase remotes/origin/tb_gl5118b_dev_v1.9_ble  //rebase指定分支

2. git push 当前所有的文件
    git add . 
        git add xx命令可以将xx文件添加到暂存区，如果有很多改动可以通过 git add -A .来一次添加所有改变的文件。注意 -A 选项后面还有一个句点。 git add -A表示添加所有内容， git add . 表示添加新文件和编辑过的文件不包括删除的文件; git add -u 表示添加编辑或者删除的文件，不包括新添加的文件
    git commit -m "提交注释"
    git push origin  分支名称，一般使用：git push origin master

1. 通过本地，把本地目录上传到github项目

    create a new repository on the command line
    [https://cloud.tencent.com/developer/article/1436151]
    (须首先在github上创建一个仓库---repostory)

    echo "# laravel-echo" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin https://github.com/lilugirl/laravel-echo.git
    git push -u origin master

    git checkout
    git tag



--------
git项目下 /opt/opencv/.git/index.lock 是做什么的

