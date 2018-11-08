# ValueType & SequenceNumber - 2018-11-07 rsy

- [模块信息](#module_info)
- [模块概要](#module_in_brief)
- [参考资料](#reference)


&nbsp;   
<a id="module_info"></a>
## 模块信息

`db/dbformat.h`

&nbsp;   
<a id="module_in_brief"></a>
## 模块概要

- `ValueType`：顺序写（sequential writing）的策略使得数据不可被更改，于是标记 “delete” 操作。占 **8** bits。在顶层调用 `Put()` 或 `Delete()` 后转发给 [WriteBatch](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/DB/WriteBatch%20-%202018-10-01%20-%20rsy.md)，在每一个 K-V `record` 中存储。
- `SequenceNumber`：标识每一次更新的版本。占 **56** bits。在 [WriteBatch](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/DB/WriteBatch%20-%202018-10-01%20-%20rsy.md) 中记录（为一批 `record`）。

> 我的理解是 SequenceNumber 是可以重复的，因为 [WriteBatch](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/DB/WriteBatch%20-%202018-10-01%20-%20rsy.md) 中是批量写入，所以每一批的 SequenceNumber 都是一样的。

[WriteBatch](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/DB/WriteBatch%20-%202018-10-01%20-%20rsy.md) 在 `InsertInto()` 中写入 memtable，`InsertInto()` 先拿出来 sequence number，然后中调用 `Iterate()` 不断迭代地将 batch 中的数据写入。

&nbsp;   
<a id="reference"></a>
## 参考资料

- [leveldb-handbook](https://leveldb-handbook.readthedocs.io/zh/latest/)
- [leveldb实现解析 - 淘宝-核心系统研发-存储](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/reference/DB%20leveldb%E5%AE%9E%E7%8E%B0%E8%A7%A3%E6%9E%90.pdf)
- [LevelDB源码分析](https://wenku.baidu.com/view/b3285278b90d6c85ec3ac687.html)