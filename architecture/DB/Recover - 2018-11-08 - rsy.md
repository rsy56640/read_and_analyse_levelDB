# Recover - 2018-11-07 - rsy

- [模块信息](#module_info)
- [模块概要](#module_in_brief)
- [模块功能](#module_function)
- [参考资料](#reference)


&nbsp;   
<a id="module_info"></a>
## 模块信息

`db/version_set.h`, `db/version_set.cc`, `db/db_impl.h`, `db/db_impl.cc`


&nbsp;   
<a id="module_in_brief"></a>
## 模块概要

数据库每次启动时，都会有一个 recover 的过程，简要地来说，就是利用 Manifest 信息重新构建一个最新的 version。


&nbsp;   
<a id="module_function"></a>
## 模块功能

![](./assets/version_recover_10_12.jpeg)

### 调用流程：

- `DB::Open()`：
  - `DBImpl::Recover()`：
      - `VersionSet::Recover()`：从 CURRENT 读 Manifest (VersionEdit)
      - 如果有 log 比 Manifest 文件中记录的 log 要新，那说明上次没来得及从内存 dump，于是从 log 回复数据，调用 `RecoverLogFile()`
      - `DBImpl::RecoverLogFile()`：打开指定的 log 文件，回放日志。期间可能会执行 compaction，产生新的 level-0-sstable文件，记录文件变动到 edit 中

> `DBImpl::RecoverLogFile()`:  
> &emsp;&emsp;&emsp;&emsp; `log_number` -> `LogFileName` -> `SequentialFile` -> `log::Reader` -> `record (Slice)` -> `WriteBatch` -> `MemTable`->   
> &emsp;&emsp;&emsp;&emsp; 对于最后一个 log，&emsp;(1) 继续用 `mem` 和 log    
> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;(2) 不继续用，就 cmpact，`WriteLevel0Table()` 并且信息存入 VersionEdit


&nbsp;   
<a id="reference"></a>
## 参考资料

- [leveldb-handbook](https://leveldb-handbook.readthedocs.io/zh/latest/)
- [leveldb实现解析 - 淘宝-核心系统研发-存储](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/reference/DB%20leveldb%E5%AE%9E%E7%8E%B0%E8%A7%A3%E6%9E%90.pdf)
- [LevelDB源码分析](https://wenku.baidu.com/view/b3285278b90d6c85ec3ac687.html)
- [跟吉姆一起读LevelDB 4.数据库恢复(1)](https://zhuanlan.zhihu.com/p/27400189)
- [跟吉姆一起读LevelDB 5.数据库恢复(2)](https://zhuanlan.zhihu.com/p/27417009)
- [跟吉姆一起读LevelDB 6.数据库恢复(3)](https://zhuanlan.zhihu.com/p/27467584)
- [leveldb - recovery](https://dirtysalt.github.io/html/leveldb.html#org97d0701)
- [leveldb源码分析—Recover和Repair](https://www.cnblogs.com/KevinT/p/3875572.html)