# bash 作协务批处理计划

## 数据库的操作
1. sh文件位置 ~/bin/
2. 创建数据库 dbc.sh
3. 应用约束 必须在某个项目（目录）下运行, 并且次目录有"db"存在
4. 运行流程
    0. 创建数据库　postgres=# CREATE DATABASE exampledb (仍用手动)
    1. 检查当前目录下有没有db目录，没有则创建
    2. 没有数据库基表创建sql文件，则从myProj/wmt/db/下复制db.sql模板 

## 用指定的用户运行命令
psql -d db1 -U userA -f /pathA/xxx.sql

## bash脚本技巧
1. 检验目录是否存在 -d
if [ ! -d "/myfolder" ]; then
  mkdir /myfolder
fi

2. 检验文件是否存在 -f
if [ ! -f "$file" ]; then
  touch "$file"
fi
