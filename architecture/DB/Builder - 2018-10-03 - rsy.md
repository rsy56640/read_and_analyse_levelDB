# Builder - 2018-10-03 rsy

- [模块信息](#module_info)
- [模块概要](#module_in_brief)
- [相关依赖说明](#dependency_specification)


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

由 `TableBuilder` 将 `MemtableIterator` 的数据依次写入。
