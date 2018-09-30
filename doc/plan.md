# Plan

**推荐使用的源码阅读工具：Source Insight / Understand (Sci-Tool)**  

项目地址：   
[rsy56640/leveldb - forked](https://github.com/rsy56640/leveldb) - 这个是我之前fork的，直接clone下来就ok    
[google/leveldb](https://github.com/google/leveldb) - 这个是最新的

提交文件放在相应目录下，文件名称：`xxx - 时间 - 作者`。    
**所有 `README` 文档均由组长完成。**   

![](assets/JIM项目结构.png)

![](assets/taobao1.png)


-----

# 所有模块均可参考 [reference](https://github.com/rsy56640/read_and_analyse_levelDB/tree/master/reference) 前5的 链接！！！！！
# 要求是：模块功能 和 对外接口 必须要弄清楚，但实现细节不要求看懂（因为各位以后都很可能不写cpp）
# 在写 模块文档 前一定要参考 [项目文档格式说明](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/doc/%E6%96%87%E6%A1%A3%E6%A0%BC%E5%BC%8F%E8%AF%B4%E6%98%8E.md)，文档格式直接copy [文档格式模板.md](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/doc/%E6%96%87%E6%A1%A3%E6%A0%BC%E5%BC%8F%E6%A8%A1%E6%9D%BF.md)

-----

## 所有 "体系结构的描述" 汇总在该模块的 `README.md` 文件下（我负责写），总体的描述我最后再写。

-----

&nbsp;   
**软件体系架构恢复**：（即本学期课程目标）

- [总体架构](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/doc/%E8%BD%AF%E4%BB%B6%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84.md)
  - [数据库逻辑架构](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/DB/README.md)
  - [底层存储架构](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/SSTable/README.md)
  - [基础工具库](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/util/README.md) ![](https://img.shields.io/badge/leveldb--util-30%25-yellow.svg)
  - [其他：平台相关-锁-信号-原子-压缩](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/%E5%B9%B3%E5%8F%B0%E7%9B%B8%E5%85%B3-%E9%94%81-%E4%BF%A1%E5%8F%B7-%E5%8E%9F%E5%AD%90-%E5%8E%8B%E7%BC%A9/README.md) 

-----

&nbsp;   
## 计划开始时间：2018-09-19   

&nbsp;   
### 09-19 ~ 09-21

全组成员安装 understand 源码阅读软件，阅读 [文档格式说明](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/doc/%E6%96%87%E6%A1%A3%E6%A0%BC%E5%BC%8F%E8%AF%B4%E6%98%8E.md)    
阅读 [参考资料](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/reference/README.md)，了解 `leveldb` 的大概结构。   

&nbsp;   
### 09-22 ~ 09-24

因为是第一次，又是中秋节，就少布置一些任务。   

1. 阅读 [这篇文档](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/doc/Understand%E9%85%8D%E7%BD%AE%E5%8F%8A%E4%BD%BF%E7%94%A8/README.md)，配置及使用 understand.
2. 阅读下面4篇文章，了解 `leveldb` 的整体构架。

<a></a>

- [LevelDB 源码分析：主体结构](http://cighao.com/2016/08/14/leveldb-source-analysis-02-structure/)  
- [LevelDB实现总结](http://morefreeze.github.io/2016/05/LevelDB-Summarize.html)
- [我的leveldb总结](https://blog.csdn.net/poi7777/article/details/49124491)
- [LevelDB详解](https://blog.csdn.net/linuxheik/article/details/52768223)


&nbsp;   
### 09-26 ~ 10-02 第五周

从一些基础工具库开始读起。

- [Status]()：杜致
  - `util/status.cc`, `include/leveldb/status.h`, 文档放置于 "DB" 文件夹下
  - 可参考：
      - [leveldb源码分析之Status](http://luodw.cc/2015/10/15/leveldb-03/)
  - 总结：返回状态，封装了错误号和错误信息，统一进行处理
- [Options, Env]()：季宇恒
  - `util/options.cc`, `include/leveldb/options.h`, `util/env.cc`, `include/leveldb/env.h`, 文档名叫 "Options.md" 和 "Env.md"
  - 指导：抽象了整个 db 的环境与配置选项，主要搞清楚接口与参数的说明
  - 可参考：
      - [leveldb](https://dirtysalt.github.io/html/leveldb.html)
      - [文件系统及Env-leveldb源码剖析(5)](http://www.pandademo.com/2016/03/file-system-and-env-leveldb-source-dissect-5/)
  - 总结：
- [Comparator]()：毛凯
  - `util/comparator.cc`, `include/leveldb/comparator.h`, `db/dbformat.h`, `db/dbformat.cc`
  - 指导：`Comparator` 下主要有两种实现：`BytewiseComparatorImpl`, `InternalKeyComparator`，区分两者的 **功能**，以及 **分别在什么情况下使用**
  - 总结：
- [ParsedInternalKey, InternalKey, LookupKey]()：苏胜
  - `db/dbformat.h`, `db/dbformat.cc`, 文档名叫 "dbformat_key.md", 文档放置于 "DB" 文件夹下
  - 指导：db 内部包装的 key 结构，需要阐释清楚 **具体结构**，**含义**，**用途** 以及 **依赖于key的操作**
  - 可参考：[leveldb实现解析 - 淘宝-核心系统研发-存储](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/reference/DB%20leveldb%E5%AE%9E%E7%8E%B0%E8%A7%A3%E6%9E%90.pdf)
  - 总结：
- [Slice](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/DB/Slice%20-%202018-09-16%20-%20rsy.md)：任思远
  - `include/leveldb/slice.h`
  - 总结：为操作数据的方便，将数据和长度包装成 Slice 使用，直接操控指针避免不必要的数据拷贝。就是个简易的 **`string`**
- [Arena](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/util/Arena/arena%20-%202018-09-30%20-%20rsy.md)：任思远
  - `util/arena.h`, `util/arena.cc`
  - 总结：Arena 主要与 MemTable 关联使用，实际主要用于 SkipList 中的 Node 内存分配，统一 MemTable 的内存分配需求，减少内存分配的实际系统调用次数（尤其针对小块内存）
- [Coding](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/util/Coding/coding%20-%202018-09-06%20-%20rsy.md)：任思远
  - `util/coding.h`
  - 总结：用于压缩 int 所占的存储空间的一套编解码工具
- [Cache](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/util/LRU/cache%20-%202018-09-20%20-%20rsy.md)：任思远
  - `include/leveldb/cache.h`, `util/cache.cc`
  - 总结：在 TableCache 用于缓存 SSTable 的对应索引元数据
- [TableCache](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/architecture/DB/TableCache%20-%202018-09-30%20-%20rsy.md)：任思远
  - `db/table_cache.h`, `db/table_cache.cc`
  - 总结：`TableCache`缓存的是 sstable 的索引数据，k-v格式为 `file_number` - `TableAndFile`
- [Iterator]()：任思远
  - `include/leveldb/iterator.h`, `table/iterator.cc`
  - 总结：leveldb 中对 key 的查找和遍历，上层统一使用 `Iterator` 的方式处理，屏蔽底层的处理，统一逻辑。 提供 `RegisterCleanup()` 可以在 `Iterator` 销毁时，做一些清理工作（比如释放 `Iterator` 持有句柄的引用）


&nbsp;   
### 10-03 ~ 10-09 第六周（演讲: 10-10）

这周看一些有趣的构件。

- [BloomFilter]()
  - `util/bloom.cc`
  - 指导：布隆过滤器的**效果**，**缺点** 以及 **何时刷新**（我猜的，可能不）
  - 可参考：
      - [BloomFilter - 基本理论](https://www.jianshu.com/p/181f40047834)
      - [布隆过滤器的原理和实现](https://github.com/cpselvis/zhihu-crawler/wiki/%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E7%9A%84%E5%8E%9F%E7%90%86%E5%92%8C%E5%AE%9E%E7%8E%B0)
  - 总结：
- [Logging]()
  - `util/logging.h`, `util/logging.cc`
  - 可参考：
  - 总结：
- [skiplist]()
  - `db/skiplist.h`, 放置于 `util/memtable` 文件夹下，这个给 **2周** 时间来看
  - 指导：跳表是工程中常见的数据结构，用于快速的kv查找，优点是比各种 BST（AVL，红黑树）易于实现，而且效率也高。主要思想是：如何在链表中维护 “某种结构” 以获得类似二分查找的性质
  - 可参考：
      - [leveldb源码分析之Skiplist](http://luodw.cc/2015/10/16/leveldb-05/)
      - [leveldb-handlebook](https://leveldb-handbook.readthedocs.io/zh/latest/memorydb.html#id2)
      - [leveldb](https://www.cnblogs.com/xueqiuqiu/tag/leveldb/)
      - [深夜学算法之SkipList：让链表飞](https://www.jianshu.com/p/fcd18946994e)
      - [leveldb(五)：SkipList跳表](https://blog.csdn.net/weixin_36145588/article/details/76393448)
- [FileNumber, FileName]()：
  - `db/filename.h`, `db/filename.cc`, `db/dbformat.h`
  - 指导：
      - db 创建文件时会按照规则将 `FileNumber` 加上特定后缀作为文件名。 所以， 运行时只需要记录 `FileNumber`（uint64_t)即可定位到具体的文件路径，省掉了字符串的麻烦。 `FileNumber` 在 db 中全局递增
      - db 中的文件用文件名区分类型
  - 可参考：
      - [leveldb实现解析 - 淘宝-核心系统研发-存储](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/reference/DB%20leveldb%E5%AE%9E%E7%8E%B0%E8%A7%A3%E6%9E%90.pdf)
      - [LevelDB源码解析11.文件序号](https://zhuanlan.zhihu.com/p/35343043)
  - 总结：
- [WriteBatch]()：任思远
  - `include/leveldb/write_batch.h`, `db/write_batch.cc`
  - 总结：
- [Table]()：任思远
  - `include/leveldb/table.h`, `table/table.cc`
  - 总结：
- [Block]()：任思远
  - `table/block.h`, `table/block.cc`
  - 总结：


&nbsp;   
### 10-10 ~ 10-16 第七周



- [FilterPolicy]()
  - `include/leveldb/filter_policy.h`
  - 指导：
  - 可参考：
      - []()
  - 总结：
- [ValueType, SequenceNumber]()：
  - `db/dbformat.h`
  - 指导：
      - `put` 插入新的 kv 数据；`delete` 是 `put` 空，为了区分真实 kv 数据和删除操作的 mock 数据，使用 `ValueType` 来标识
      - leveldb 中的每次更新（`put`/`delete`)操作都拥有一个版本，由 `SequnceNumber` 来标识，整个 db 有一个全局值保存着当前使用到的 `SequnceNumber`
  - 可参考：[leveldb实现解析 - 淘宝-核心系统研发-存储](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/reference/DB%20leveldb%E5%AE%9E%E7%8E%B0%E8%A7%A3%E6%9E%90.pdf)
  - 总结：
- []()：
  - ``
  - 指导：
  - 可参考：
      - []()
  - 总结：
- []()：
  - ``
  - 指导：
  - 可参考：
      - []()
  - 总结：
- []()：
  - ``
  - 指导：
  - 可参考：
      - []()
  - 总结：


&nbsp;   
### 10-17 ~ 10-23 第八周

- []()：
  - ``
  - 指导：
  - 可参考：
      - []()
  - 总结：
- []()：
  - ``
  - 指导：
  - 可参考：
      - []()
  - 总结：
- []()：
  - ``
  - 指导：
  - 可参考：
      - []()
  - 总结：
- []()：
  - ``
  - 指导：
  - 可参考：
      - []()
  - 总结：
- []()：
  - ``
  - 指导：
  - 可参考：
      - []()
  - 总结：

