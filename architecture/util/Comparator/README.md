# Module - 2018-10-10 毛恺

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
Comparator(util/comparator.cc,include/leveldb/comparator.h，db/dbformat.h，db/dbformat.cc)


&nbsp;   
<a id="module_in_brief"></a>
## 模块概要
Comparator:
leveldb是按Key序组织的,对于Key的比较非常重要。
Comparator的主要作用就是Key的比较，同时也会对key进行一定的优化，以降低一些存储空间。

BytewiseComparatorImpl&InternalKeyComparator:
在dbformat中实现的两种Comparator。

&nbsp;   
<a id="module_function"></a>
## 模块功能



&nbsp;   
<a id="interface_specification"></a>
## 接口说明




&nbsp;   
<a id="dependency_specification"></a>
## 相关依赖说明



&nbsp;   
<a id="inner_detail"></a>
## 内部实现细节



&nbsp;   
<a id="reference"></a>
## 参考资料

