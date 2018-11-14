# DBIter - 2018-11-14 - rsy

- [模块信息](#module_info)
- [模块概要](#module_in_brief)
- [接口说明](#interface_specification)
- [相关依赖说明](#dependency_specification)
- [内部实现细节](#inner_detail)
- [参考资料](#reference)


&nbsp;   
<a id="module_info"></a>
## 模块信息

`db/db_iter.cc`, `db/db_iter.h`


&nbsp;   
<a id="module_in_brief"></a>
## 模块概要

封装 db 迭代器


&nbsp;   
<a id="interface_specification"></a>
## 接口说明

`Iterator* NewDBIterator(DBImpl*, Iterator*, SequenceNumber)`


&nbsp;   
<a id="dependency_specification"></a>
## 相关依赖说明

- `DBImpl::NewIterator()`
  - 调用 `DBImpl::NewInternalIterator(SequenceNumber*)` 生成 `internal_iter`
  - 调用 `NewDBIterator(internal_iter)` 生成遍历 db 的迭代器

&nbsp;   
<a id="inner_detail"></a>
## 内部实现细节

- `DBIter::Next()`：
- `DBIter::Prev()`：
- `DBIter::Seek(const Slice& target)`：
- `DBIter::SeekToFirst()`：
- `DBIter::SeekToLast()`：

这个地方挺奇怪，大致意思就是有很多 key 重复，要跳过。   
不过实现没太搞懂。。。

- `DBIter::FindNextUserEntry(bool skipping, std::string* skip)`：
- `DBIter::FindPrevUserEntry()`：


&nbsp;   
<a id="reference"></a>
## 参考资料

- [leveldb - handbook](https://leveldb-handbook.readthedocs.io/zh/latest/)
- [leveldb](https://dirtysalt.github.io/html/leveldb.html)
- [leveldb实现解析 - 淘宝-核心系统研发-存储](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/reference/DB%20leveldb%E5%AE%9E%E7%8E%B0%E8%A7%A3%E6%9E%90.pdf)
- [LevelDB源码分析](https://wenku.baidu.com/view/b3285278b90d6c85ec3ac687.html)