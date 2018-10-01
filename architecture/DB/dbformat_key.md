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

InternalKey(db/dbformat.h,db/dbformat.cc)
 
LookupKey(db/dbformat.h,db/dbformat.cc)

&nbsp;   
<a id="module_in_brief"></a>
## 模块概要
1.ParsedInternalKey是db内部操作的key。
 
2.InternalKeydb内部，包装易用的结构，包含userkey与SequnceNumber/ValueType。

3.LookupKey是db内部在为查找memtable/sstable方便，包装使用的key结构，保存有userkey与SequnceNumber/ValueType dump 在内存的数据。
   
<a id="module_function"></a>
## 模块功能

![](assets/ParsedInternalKey_10_01.png)

ParsedInternalKey（struct ParsedInternalKey）就是对 InternalKey 分拆后的结果，先来看看 ParsedInternalKey 的成员，

这是一个 struct{

Slice user_key; 

SequenceNumber sequence; 

ValueType type;
}
 
![](assets/InternalKey_10_01.png)

class InternalKey 是一个只存储了一个 string，它使用一个 DecodeFrom() 函数将 Slice 类型的 InternalKey 解码出 string 类型的 InternalKey。  

void DecodeFrom(const Slice& s) { rep_.assign(s.data(), s.size()); } 
 
也就是说 InternalKey 是由 User_key.data + SequenceNumber + ValueType组合而成的。

InternalKey 的格式为： | User_key.data (string)| sequence number (7 B) | value type (1 B) | 

![](assets/LookupKey_10_01.png)


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

