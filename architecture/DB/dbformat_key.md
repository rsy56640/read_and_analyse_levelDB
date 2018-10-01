# Module - 2018-10-01 苏胜

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
ParsedInternalKey(db/dbformat.h,db/dbformat.cc)
&nbsp; 
InternalKey(db/dbformat.h,db/dbformat.cc)
&nbsp; 
LookupKey(db/dbformat.h,db/dbformat.cc)

&nbsp;   
<a id="module_in_brief"></a>
## 模块概要
1.ParsedInternalKey是db内部操作的key。
&nbsp; 
2.InternalKeydb内部，包装易用的结构，包含userkey与SequnceNumber/ValueType。
&nbsp; 
3.LookupKey是db内部在为查找memtable/sstable方便，包装使用的key结构，保存有userkey与SequnceNumber/ValueType dump 在内存的数据。
&nbsp;   
<a id="module_function"></a>
## 模块功能
![Image read_and_analyse_levelDB/architecture/DB/assets/InternalKey_10_01.png]


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

