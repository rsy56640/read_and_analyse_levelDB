# BloomFilter - 2018-10-10 毛恺

- [模块信息](#module_info)
- [模块概要](#module_in_brief)
- [模块功能](#module_function)
- [接口说明](#interface_specification)
- [参考资料](#reference)


&nbsp;   
<a id="module_info"></a>
## 模块信息

BloomFilter(util/bloom.cc)

&nbsp;   
<a id="module_in_brief"></a>
## 模块概要
leveldb采用分层储存结构，查询时最差的结果是在所有的层（level）上都查询一次。

Bloom Filter是一种空间效率很高的随机数据结构，可以使用较少的空间存储大量数据的全集，它利用位数组很简洁地表示一个集合，并能判断一个元素是否属于这个集合。

这样做的代价是：再判断一个元素是否属于一个集合时，有可能会错把其他元素误以为是该集合元素。
当追求“零错误”时，BloomFilter不适用。

&nbsp;   
<a id="module_function"></a>
## 模块功能
![](assets/BloomFilterPolicy.png)

CreateFilter(const Slice* keys, int n, std::string* dst)
将传入的n个key 存储到bloomfilter 中，bloomfilter结果使用string存储。

KeyMayMatch(const Slice* keys,const Slice& bloom_filter)
判断一个 key 在bloomfilter中是否存在。

BloomFilterPolicy(int bits_per_key)
按照公式设定hash函数的个数

&nbsp;   
<a id="interface_specification"></a>
## 接口说明

构造函数：
explicit BloomFilterPolicy(int bits_per_key)
没有默认构造函数，使用时必须传入bits_per_key，表示每个key的大小


&nbsp;   
<a id="reference"></a>
## 参考资料

Bloom Filter概念和原理：https://blog.csdn.net/jiaomeng/article/details/1495500