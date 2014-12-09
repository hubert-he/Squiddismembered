Squiddismembered
================

squid代码分解成若干部分

default_squid_conf
=================
make all 生成squid 需要的默认配置文件squid.conf.default，此文件在squidmain函数中初始化期间载入并初始化config全局变量，
然后载入真正的配置文件，对比进行修改config全局变量的值。
