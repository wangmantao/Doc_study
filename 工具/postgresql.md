# Postgresql 学习笔记

## 灵感


## 知识点点滴
24. 获取某表的字段集
    SELECT * FROM information_schema.columns WHERE table_schema = 'public'  AND table_name = 'shop';
    (以上是表的所有信息，其中包含字段名列“column_name)

23. csv到数据库
    https://stackoverflow.com/questions/2987433/how-to-import-csv-file-data-into-a-postgresql-table
    COPY zip_codes FROM '/path/to/csv/ZIP_CODES.txt' WITH (FORMAT csv);

    \copy zip_codes FROM '/path/to/csv/ZIP_CODES.txt' DELIMITER ',' CSV
        You can also specify the columns to read:

    \copy zip_codes(ZIP,CITY,STATE) FROM '/path/to/csv/ZIP_CODES.txt' DELIMITER ',' CSV
    
       PGPASSWORD=postgres psql -Upostgres -d inout -c "\copy customers FROM '/home/wmt/myProj/yuanLiang/qt/inout/cmd/output.csv' DELIMITER ',' CSV"

22. 关于用户名和密码
    本机 postgres :
                1. 作为系统的用户
                    wmt (passwd 可修改)
                2. 作为数据库的用户
                    postgres (?? 可修改)
                    用于psql命令行操作数据时

21. 把数据库表导出csv
    总结两种运行方法：
        1. 在psql提示符下，直接运行  (纠正：也可用psql -c "copy ...")
            COPY customers TO '/var/lib/pgsql/test.csv' DELIMITER ',' CSV HEADER
        2. 写psql 命令连用   (以上已经纠正该误解)
            psql -d dbname -t -A -F"," -c "select * from users" > output.csv
            (-t 只要元组数据，不要标题)
            PGPASSWORD=postgres psql -Upostgres -d inout  -A -F"," -c "select * from customers" > output.csv

    COPY customers TO '/var/lib/pgsql/test.csv' DELIMITER ',' CSV HEADER
    (以上不同于sql命令，最后不带分号)
    http://www.postgresqltutorial.com/export-postgresql-table-to-csv-file/
    网页有讲：
        可以导入scv数据到表
        可以从远程导出scv 到本地(\copy)
        \copy (SELECT * FROM locations) to '/home/wmt/temp/aa.csv' with csv
    QT中却不能这么做,原因：https://stackoverflow.com/questions/31309181/unable-to-create-query-copy-postgresql-pqsql-driver
    QT可以笨重的方法： QT 通用数据库数据导入导出方案 https://tcspecial.iteye.com/blog/1897110
    windows可否 psql -d dbname -t -A -F"," -c "select * from users" > output.csv
        (用法说明：https://stackoverflow.com/questions/1517635/save-pl-pgsql-output-from-postgresql-to-a-csv-file?noredirect=1)
        (windows 下的命令：windows下怎么打开psql命令)

20. 连接数据问题
    不同线程共用一个连接，可能导致问题
    postgresql.conf 配置文件中的 max_connections 参数可设置连接数
    Database.cpp 多建几个连接连连车链接kj

19. postgres 用户密码更改
    alter user postgres with password 'postgres';

18. 对用户"postgres"的对等认证失败 (用psql直接运行sql)
    https://www.cnblogs.com/ibgo/p/5961849.html
    vi    /var/lib/pgsql/data/pg_hba.conf (要是postgres用户)
    local all all peer -> local all all md5
    重启服务器

17. 新建数据库表　“sysState"
    sn A_time V1_time V2_time password

16. 数据库覆盖
    systemctl stop postgresql
    /var/lib/pgsql/data 改名
    postgresql-setup initdb
    systemctl start postgresql
    psql -f 两个备份文件 

15. 数据库备份 (导出)
    su postgres （需要切换到系统的postgres用户来进行备份）

    pg_dumpall > ~/back/all_the_data.backup （备份整个数据库集群）
    pg_dump bats -f ~/back/db.backup

    psql -f ~/back/all_the_data.backup 
    psql -f ~/back/db.backup 



14. 在数据库中加字段 sql语句
    ALTER TABLE dingtalk_corp_info ADD COLUMN IF NOT EXISTS admin_id TEXT;

13. Postgres push triggers 实验
    1. 监视一个表，并发送notifications 给通道 "changeFeed" 
        select watch_table(users, 'changeFeed');
        --- 执行这个函数的意思，参１表名, 参２通道名

    2. 监听这个通道，异步执行
        listen changeFeed;

    3. 做实验触发notification
        insert into 表名　values (1, 'joe');

    4. shell出现了notification...... 

12. Postgres push triggers 
    https://gist.github.com/bruth/6d53a3c2138c5adf53f5

    NOTIFY SQL Commands   
    https://www.postgresql.org/docs/current/sql-notify.html

11. postgres 关建技术
        查询用户成员
        select * from pg_roles;
        select * from pg_user;

        权限查询：
        select * from information_schema.table_privileges where grantee='cc';

        让远程登录
        postgressql 忘记密码远程不能登录 (port default: 5432)
        /var/lib/pgsql/data/pg_hba.conf
       host    all all 127.0.0.1/32 trust
             sudo systemctl restart postgresql
              psql -U postgres -h 127.0.0.1 bats (密码都不需要)

        /var/lib/pgsql    (这是postgres用户的家目录)

     改密码：
    postgres=# ALTER USER admin PASSWORD 'admin';
   　松下笔记本的用户名：postgres
                密码: admin
    服务启动方法: postgresql
    su postgres   (进入这个特别的用户
    pg_ctl start  (再执行手动启动


10.  执行sql文件
    \i *.sql
    
9. linux 下安装数据库
   yum install postgresql-server.x86_64 postgresql-contrib.x86_64 
       
8. max min的认识
  以前以为max/min只能用在number里
　其实可以用在其它类型列


7. partition 数据分组 
 function name over (partition by column)   这里的partition by 子句用于对行进行分组的。

6. 窗口函数语法
windFn() over() from 集合
求单值，却返回与集合一样多的rows
	
5. 窗口函数
“ The whole idea behind window functions is :
to allow you to process several values of the result set at a time:
(充许你处理返回集合中的值)
 you see through the window some peer rows and are able to compute a single output value from them, 
通过匹配的行，并且可以计算它们的单个值输出,
much like when using an aggregate function. ”
类似聚合（合计）函数．

4. jdbc 查询
	(jdbc/query 连接名 ["select查义字串"]) ;; 连接名是map变量

1. pgsql 连接远程服务器
　　psql -h IP地址 -p 端口  -U 
    psql -h www.taily.cc -p 5432 english admin
    [pali dbName](~/bin/pali 快捷命令)

2. 查看数据库/改变目标数据库
	\l 查看有哪些数据库
	\d 查看有哪些表
	\d 表名　查看表结构(字段)
3. 建个视图/并查询它

## 灵感 (备份)
1. 全部终端操作，不和GUI 
	容易掉线，sali远程操作吧

