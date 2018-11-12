# Logging - 2018-10-14 季宇恒

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
util/logging.cc
util/logging.h

&nbsp;   
<a id="module_in_brief"></a>
## 模块概要
leveldb中启动时的一些配置通过Option传入，get/put/delete时，也有相应的ReadOption/WriteOption
   
<a id="module_function"></a>
## 模块功能

将可读的“num”打印输出附加到*str
将可读的“值”打印输出附加到*str。
转义“值”中的任何不可打印字符。
返回一个人类可读的“值”版本。
转义“值”中的任何不可打印字符。
将人类可读的数字从“*in”解析为*值。成功，将“*”提前到已消费的数字，并将“*val”设置为数字值。否则，返回false并在其中留下*未指明的状态。

&nbsp;   
<a id="interface_specification"></a>
## 接口说明
Logger接口非常简单，就是Logv.允许打印变长参数.实现是PosixLogger. 

class Logger {
 public:
  Logger() { }
  virtual ~Logger();

  // Write an entry to the log file with the specified format.
  virtual void Logv(const char* format, va_list ap) = 0;

 private:
  // No copying allowed
  Logger(const Logger&);
  void operator=(const Logger&);
};

&nbsp;   
<a id="dependency_specification"></a>
## 相关依赖说明
需要Slice类型的定义和用法
Slice (include/leveldb/slice.h)
需要Env类型的定义和用法
Slice (include/leveldb/env.h)

&nbsp;   
<a id="inner_detail"></a>
## 内部实现细节

&nbsp;   
<a id="reference"></a>
## 参考资料
- [leveldb实现解析 - 淘宝-核心系统研发-存储](https://github.com/rsy56640/read_and_analyse_levelDB/blob/master/reference/DB%20leveldb%E5%AE%9E%E7%8E%B0%E8%A7%A3%E6%9E%90.pdf)
- [LevelDB源码分析 - 百度文库 100多页..................](https://wenku.baidu.com/view/b3285278b90d6c85ec3ac687.html)
