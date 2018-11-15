# TableCache - 2018-09-30 rsy


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

TableCache (`db/table_cache.cc`, `db/table_cache.h`)   


&nbsp;   
<a id="module_in_brief"></a>
## 模块概要

`TableCache` 缓存 `Table` 对象，每个DB一个。

`TableCache` 的 KV 格式：

- 以 `file_number` 作 key
- 以 `TableAndFile` 对应的预加载索引作为 value。

&nbsp;   
<a id="module_function"></a> 
## 模块功能

![](assets/TableCache_UML_09_26.png)

table cache 缓存的是 sstable 的索引数据

来看 `TableCache`，先简单梳理下到 `TableCache` 的查找流程，用户提交key查询，交由 `Status DBImpl::Get()` ，获取两种 `MemTable` 和当前 `Version`，依次查询 `memtable`、`immutable memtable` ，未找到则在当前 `Version`上 `Status Version::Get()`，依次从最低level到最高level查询直至查到，在每层确定可能包含该key的 SSTable 文件后，就会在所属 `VersionSet` 的 `table_cache` 中继续查询，即调用 `Status TableCache::Get()`

另外，`TableCache` entry 的插入在 Compaction 时也有体现，每当通过 Compatction 生成新的 SSTable 文件，也会以验证正常可用的方式更新该 SSTable 的索引信息到 `TableCache`。


&nbsp;   
<a id="interface_specification"></a>
## 接口说明

- Evict(文件号)：清除指定文件所有的 cache 和 entry
- Get()：这是一个查找函数，如果在指定文件中 seek 到 internal key "k" 找到一个 entry，就调用 `(*handle_result)(arg, found_key, found_value)`
- NewIterator()：为指定的 file 返回一个 `iterator`，


&nbsp;   
<a id="dependency_specification"></a>
## 相关依赖说明

被 `class DBImpl` private含有




&nbsp;   
<a id="inner_detail"></a>
## 内部实现细节

`TableCache::Get()`:   
首先根据 `file_number` 找到 Table 的 cache 对象（获取该 key 所属 SSTable 的句柄后，调用 `Status Table::InternalGet()` 查找真正包含该 key 的数据块），如果找到了就调用 `Table::InternalGet()`，对查找结果的处理在调用者传入的 saver 回调函数中。Cache 在 Lookup 找到 cache 对象后，如果不再使用需要调用 `Release()` 减引用计数。
>**疑惑**：为什么最后要进行 `Release()`，可能和 TwoLevelCache 的读取和更新有关，回头补上。。。


&nbsp;   
`TableCache::NewIterator()`：   
调用 `FindTable()`，返回 `Handle` 对象，取出 `Table` 对象，并创建一个 `Iterator`，并注册一个回调的 cleanup 函数。（**实际上经常用于添加 sstable 到 TableCache**）


&nbsp;    
`TableCache::Evict()`：   
根据 `file_number` 清除 cache 对象。


&nbsp;   
`TableCache::FindTable()`：   
先根据 `file_number` 在 cache 中查

- 如果有，返回ok
- 否则（即未命中）创建 `TableAndFile`，即 `RandomAccessFile` 和 `Table`（`Table` 文件格式为 <db_name><file_number%6>），然后调用 `Table::Open()`（并将.sst文件映射到table，Table类用于解析.sst文件） 并加入缓存。



&nbsp;   
<a id="reference"></a>
## 参考资料

- [leveldb源码分析之Table_cache](http://luodw.cc/2015/10/25/leveldb-13/)
- [levelDB源码分析-TableCache](https://blog.csdn.net/tankles/article/details/7663929)
- [LevelDB源码分析之十五：table cache](https://blog.csdn.net/caoshangpa/article/details/79062784)
- [SSTable之两级Cache-leveldb源码剖析(8)](http://www.pandademo.com/2016/04/two-stage-cache-of-sstable-leveldb-source-dissect-8/)
- [Leveldb源码分析--11](https://blog.csdn.net/sparkliang/article/details/8740712)