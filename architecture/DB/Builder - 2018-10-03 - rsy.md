# Builder - 2018-10-03 rsy

- [模块信息](#module_info)
- [模块概要](#module_in_brief)
- [相关依赖说明](#dependency_specification)
- [内部实现细节](#inner_detail)


&nbsp;   
<a id="module_info"></a>
## 模块信息

`db/builder.h`, `db/builder.cc`


&nbsp;   
<a id="module_in_brief"></a>
## 模块概要

`BuildTable()`：为 level-0 创建 `Table`，并把 metadata 存到 `FileMetaData* meta`


&nbsp;   
<a id="dependency_specification"></a>
## 相关依赖说明

由 `TableBuilder` 将 `MemtableIterator` 的数据依次写入 sstable，并将新生成的 sstable 加入 TableCache。


&nbsp;   
<a id="inner_detail"></a>
## 内部实现细节

    extern Status BuildTable(const std::string& dbname,
                             Env* env,
                             const Options& options,
                             TableCache* table_cache,
                             Iterator* iter,
                             FileMetaData* meta);

`dbname` -> `fname` -> `WritableFile*` -> `TableBuilder*` -> 循环 `iter`，`TableBuilder::Add(iter->key(), iter->value())` -> `TableBuilder::Finish()`