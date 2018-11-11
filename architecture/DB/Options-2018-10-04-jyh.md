# Options - 2018-10-04 季宇恒

- [模块信息](#module_info)
- [模块概要](#module_in_brief)
- [模块功能](#module_function)
- [接口说明](#interface_specification)
- [相关依赖说明](#dependency_specification)
- [内部实现细节](#inner_detail)
- [参考资料](#reference)


&nbsp;   
<a id="module_info"></a>
## 模块信息

util/options.cc

include/leveldb/options.h

&nbsp;   
<a id="module_in_brief"></a>
## 模块概要
leveldb中启动时的一些配置通过Option传入，get/put/delete时，也有相应的ReadOption/WriteOption

&nbsp;   
<a id="module_function"></a>
## 模块功能

控制传入的comparator，open时如果db目录不存在就创建，存在就报错
控制是否保存中间的错误状态，用compact时是否读到的block做检验
控制传入的env,打印日志的logger，memtable的最大size
控制db中打开文件的最大个数，db中需要打开的文件包括基本的CURRENT/LOG/MANIFEST/LOCK,以及打开的sstable文件
控制传入block数据的cache管理
控制对size的管理
block中对key做前缀压缩

&nbsp;   
<a id="interface_specification"></a>
## 接口说明

1.leveldb::Options
传入comparator，open时如果db目录不存在就创建，存在就报错
sstable一旦打开就会将index信息加入TableCache，所以把(max_open_files-10)作为tablecache的最大数量

2.leveldb::ReadOptions
是否对读到的block做检验，读到的block是否加入block cache，指定读取的SnapShot

3.leveldb::WriteOptions
write时，记binlog之后，是否对binlog做sync，如果传入不为NULL，write完成之后同时做SnapShot

&nbsp;   
<a id="dependency_specification"></a>
## 相关依赖说明
需要Export类型的定义和用法
Export (include/leveldb/export.h)
操作需要返回值

&nbsp;   
<a id="inner_detail"></a>
## 内部实现细节

&nbsp;   
<a id="reference"></a>
## 参考资料
- [leveldb实现解析 - 淘宝-核心系统研发-存储](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/reference/DB%20leveldb%E5%AE%9E%E7%8E%B0%E8%A7%A3%E6%9E%90.pdf)
