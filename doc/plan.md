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
# 在写 模块文档 前一定要参考 [项目文档格式说明](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/doc/%E6%96%87%E6%A1%A3%E6%A0%BC%E5%BC%8F%E8%AF%B4%E6%98%8E.md)

-----

## 计划开始时间：2018-09-19   

### 09-19 ~ 09-21

全组成员安装 understand 源码阅读软件，阅读 [文档格式说明](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/doc/%E6%96%87%E6%A1%A3%E6%A0%BC%E5%BC%8F%E8%AF%B4%E6%98%8E.md)    
阅读 [参考资料](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/reference/README.md)，了解 `leveldb` 的大概结构。   

### 09-22 ~ 09-24

因为是第一次，又是中秋节，就少布置一些任务。   

1. 阅读 [这篇文档](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/doc/Understand%E9%85%8D%E7%BD%AE%E5%8F%8A%E4%BD%BF%E7%94%A8/README.md)，配置及使用 understand.
2. 阅读下面4篇文章，了解 `leveldb` 的整体构架。

<a></a>

- [LevelDB 源码分析：主体结构](http://cighao.com/2016/08/14/leveldb-source-analysis-02-structure/)  
- [LevelDB实现总结](http://morefreeze.github.io/2016/05/LevelDB-Summarize.html)
- [我的leveldb总结](https://blog.csdn.net/poi7777/article/details/49124491)
- [LevelDB详解](https://blog.csdn.net/linuxheik/article/details/52768223)


### 09-26 ~ 10-02

从一些基础工具库开始读起。

- [Status]()：杜致
  - `util/status.cc`, `include/leveldb/status.h`, 文档放置于 "DB" 文件夹下
  - 可参考：
- [Options, Env]()：季宇恒
  - `util/options.cc`, `include/leveldb/options.h`, `util/env.cc`, `include/leveldb/env.h`, 文档名叫 "Options.md" 和 "Env.md"
  - 可参考：
- [Comparator]()：毛凯
  - `util/comparator.cc`, `include/leveldb/comparator.h`
  - 可参考：
- [ParsedInternalKey, InternalKey, LookupKey]()：苏胜
  - `db/dbformat.h`, `db/dbformat.cc`, 文档名叫 "dbformat_key.md", 文档放置于 "DB" 文件夹下
  - 可参考：
- [Arena]()：任思远
  - `util/arena.h`, `util/arena.cc`
  - 可参考：
- [TableCache]()：任思远
  - `db/table_cache.h`, `db/table_cache.cc`
  - 可参考：



### 10-03 ~ 10-09

这周看一些有趣的构件。

- [BloomFilter]()
  - `util/bloom.cc`
  - 可参考：
- [Logging]()
  - `util/logging.h`, `util/logging.cc`
  - 可参考：
- [skiplist]()
  - `db/skiplist.h`, 放置于 `util/memtable` 文件夹下，这个给 **2周** 时间，来看，之后接 `memtable`
  - 可参考：
- [Iterator]()
  - `include/leveldb/iterator.h`, `table/iterator.cc`
  - 可参考：
- [WriteBatch]()：任思远
  - `include/leveldb/write_batch.h`, `db/write_batch.cc`
  - 可参考：
- [Table]()：任思远
  - `include/leveldb/table.h`, `table/table.cc`
  - 可参考：
- [Block]()：任思远
  - `table/block.h`, `table/block.cc`
  - 可参考：


### 10-10 ~ 10-16



- [FilterPolicy]()
  - `include/leveldb/filter_policy.h`
  - 可参考：
- []()
- []()
- []()
- []()



