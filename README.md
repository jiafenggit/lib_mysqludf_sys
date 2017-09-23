1.解压文件，在源码目录里编译源代码：

gcc -DMYSQL_DYNAMIC_PLUGIN -fPIC -Wall -I/usr/include/mysql -I. -shared lib_mysqludf_sys.c -o lib_mysqludf_sys.so
 

2.把lib_mysqludf_sys.so文件放到 mysql的lib/mysql/plugin/



3.在mysql中执行如下sql创建函数


DROP FUNCTION IF EXISTS lib_mysqludf_sys_info;
DROP FUNCTION IF EXISTS sys_get;
DROP FUNCTION IF EXISTS sys_set;
DROP FUNCTION IF EXISTS sys_exec;
DROP FUNCTION IF EXISTS sys_eval;
 
CREATE FUNCTION lib_mysqludf_sys_info RETURNS string SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_get RETURNS string SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_set RETURNS int SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_exec RETURNS int SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_eval RETURNS string SONAME 'lib_mysqludf_sys.so';


4.测试

    创建shell脚本


vi /var/ant/ant_msg.sh
 
#/bin/sh
date > /tmp/testlog.txt



执行SQL: SELECT sys_exec("/var/ant/ant_msg.sh")


    

检查日志: tail -f /tmp/testlog.txt
