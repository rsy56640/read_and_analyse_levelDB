# LevelDB

![](assets/leveldb_icon_09_26.png)

## Abstract
LevelDB is an open source on-disk key-value store written by Google fellows Jeffrey Dean and Sanjay Ghemawat. Inspired by Bigtable, LevelDB is hosted on GitHub under the New BSD License and has been ported to a variety of Unix-based systems, Mac OS X, Windows, and Android.

-----

## Table of Contents

- [1. Introduction](#Introduction)
- [2. Stakeholders](#stakeholders)
- [3. Context View](#Context_View)
  - [3.1 leveldb in Bigtable](#leveldb_in_Bigtable)
  - [3.2 Platform & Dependency](#Platform_Dependency)
  - [3.3 Development & Community](#Development_Community)
- [4. Functional View](#Functional_View)
- [5. Architecture](#Architecture)
  - [5.1 Module View](#Module_View)
  - [5.2 Common Design Models](#Common_Design_Models)
- [6. Evolution Perspective](#Evolution_Perspective)
- [7. Technical Debt Analysis](#Technical_Debt_Analysis)
- [8. Conclusion](#Conclusion)
- [9. References](#References)



&nbsp;   
<a id="introduction"></a>
## 1. Introduction

LevelDB is based on concepts from Google's Bigtable database system. The table implementation for the Bigtable system was developed starting in about 2004, and is based on a different Google internal code base than the LevelDB code. That code base relies on a number of Google code libraries that are not themselves open sourced, so directly open sourcing that code would have been difficult. Jeff Dean and Sanjay Ghemawat wanted to create a system resembling the Bigtable tablet stack that had minimal dependencies and would be suitable for open sourcing, and also would be suitable for use in Chrome for the IndexedDB implementation. They wrote LevelDB starting in early 2011, with the same general design as the Bigtable tablet stack, but not sharing any of the code.

### Features

- Keys and values are arbitrary byte arrays.
- Data is stored sorted by key.
- Callers can provide a custom comparison function to override the sort order.
- The basic operations are `Put(key,value)`, `Get(key)`, `Delete(key)`.
- Multiple changes can be made in one atomic batch.
- Users can create a transient snapshot to get a consistent view of data.
- Forward and backward iteration is supported over the data.
- Data is automatically compressed using the [Snappy compression library](http://google.github.io/snappy/).
- External activity (file system operations etc.) is relayed through a virtual interface so users can customize the operating system interactions.


&nbsp;   
<a id="stakeholders"></a>
## 2. Stakeholders

To start off our analysis, we will look at the stakeholders involved with levelDB. A stakeholder is an entity of a system architecture that consists of an individual or an organization that has importance and interest to realize a system.

LevelDB is used as the backend database for Google Chrome's IndexedDB and is one of the supported backends for Riak. Additionally, Bitcoin Core stores the blockchain metadata using a LevelDB database. Minecraft: Pocket Edition uses a modified version for chunk and entity data storage. Autodesk AutoCAD 2016 also uses LevelDB.   

### [levelDB is used in Bitcoin](https://github.com/bitcoin/bitcoin/tree/master/src/leveldb)

Bitcoin uses LevelDB to store and retrieve block information.

On the subject of why LevelDB is used, core developer Greg Maxwell stated the following to the bitcoin-dev mailing list in October 2015:
>I think people are falling into a trap of thinking "It's a <database>, I know a <black box> for that!"; but the application and needs are very specialized here. . . It just so happens that on the back of the very bitcoin specific cryptographic consensus algorithim there was a slot where a pre-existing high performance key-value store fit; and so we're using one and saving ourselves some effort...

[How is Bitcoin using LevelDB? - Quroa](https://www.quora.com/How-is-Bitcoin-using-LevelDB)   
[Bitcoin Core 0.11 (ch 2): Data Storage](https://en.bitcoin.it/wiki/Bitcoin_Core_0.11_(ch_2):_Data_Storage)   

### [levelDB is used in frontend](http://leveldb.org/)

[Node.js support package on Github](https://github.com/Level/level)

The Node.js Community built [level](https://github.com/Level/level) (Fast & simple storage - a Node.js-style LevelDB wrapper).

[Richard Astbury](https://coderead.wordpress.com/2013/04/04/node-js-leveldb/):
>This article is based on a recent NodeUp podcast dedicated to LevelDB, I recommend listening to it.   
I have played around a bit with Level, and I’ve been really impressed by it’s speed, ease and simplicity. You should give it a go.

![](assets/SA_StakeHolder_10_09.png)

![](assets/SA_Stakeholder_sponsors_level_10_09.png)

### [levelDB is used in Facebook](https://rocksdb.org/)

RocksDB is a high performance embedded database for key-value data. It is a fork of LevelDB by Facebook optimized to exploit many central processing unit (CPU) cores, and make efficient use of fast storage, such as solid-state drives (SSD), for input/output (I/O) bound workloads.


&nbsp;   
<a id="context_view"></a>
## 3. Context View

leveldb is a BigTable-inspired key-value database library. It’s available for Unix based systems, Mac OS X, Windows, and Android… LevelDB is not a database server like other other key-value stores like Redis or Membase. Instead, it would most likely be used as an embedded database for other applications, much the way SQLite or Berkley DB are used. The technical advantage to using LevelDB instead of other key-value stores is its support for ordered data. Also, its BSD license is more liberal than the GPL Sleepycat license of Berkley DB.”


&nbsp;   
<a id="leveldb_in_bigtable"></a>
### 3.1 leveldb in Bigtable

Bigtable is built on several other pieces of Google infrastructure such as levelDB and the distributed Google File System (GFS).   
The levelDB SSTable file format is used internally to
store Bigtable data. An SSTable provides a persistent,
ordered immutable map from keys to values, where both
keys and values are arbitrary byte strings. Operations are provided to look up the value associated with a specified key, and to iterate over all key/value pairs in a specified key range. Internally, each SSTable contains a sequence of blocks (typically each block is 64KB in size, but this is configurable). A block index (stored at the end of the SSTable) is used to locate blocks; the index is loaded into memory when the SSTable is opened. A lookup can be performed with a single disk seek: we first find the appropriate block by performing a binary search in the in-memory index, and then reading the appropriate block from disk. Optionally, an SSTable can be completely mapped into memory, which allows us to perform lookups and scans without touching disk.

![](assets/SA_Bigtable_10_09.jpg)


<a id="platform_dependency"></a>
### 3.2 Platform & Dependency

#### [Levelup](https://github.com/Level/levelup)
Levelup is a Node.js project that aims to provide a common, portable interface to a multitude of LevelDB forks such as Hyperdex. Check out the many flavours of LevelDB made available though this project.

#### Embeddable & Networkable
LevelDB is embedded, but can be networked adding protocols such as http, tcp or udp to your process.


<a id="development_community"></a>
### 3.3 Development & Community

A platform that plays an important role during development is GitHub, which is used for code versioning, issue tracking and project management.[leveldb on Github](https://github.com/google/leveldb)

Discussion group for LevelDB. LevelDB is a open source library that implements a fast persistent key-value store.[leveldb Group on Google Forum](https://groups.google.com/forum/#!forum/leveldb)


&nbsp;   
<a id="functional_view"></a>
## 4. Functional View

LevelDb is the two great spirit level engineers is an open-source project, in short, LevelDb is capable of processing one billion scale Key-Value persistent data storage C++ Library. As described above, the two is the design and implementation of Bigtable, if the understanding of Bigtable, should know that there are two core in distributed storage system, the influence of part: Master Server and Tablet Server. The Master Server store data management and distributed scheduling, the distributed data storage and read and write operation is completed by the Tablet Server, and LevelDb can be understood as a simplified version of the Tablet Server.

LevelDb has the following advantages:

- First of all, LevelDb is a k-v system for persistent storage, k-v system and Redis this type of memory is different, LevelDb doesn't like Redis eat memory, but most of the data stored to disk.
- Secondly, LevleDb when storing data, is based on the records of key value orderly storage, is adjacent to the key value in the storage file is stored in sequence, and the application can customize the key size comparison function, LevleDb will be in accordance with the comparison of user defined function sequentially stored the records.
- Again, like most k-v systems, the operation interface of LevelDb is very simple, basic operation including written records, delete records recording and reading. Support for multiple operating atomic batch operation.
- In addition, LevelDb supports data snapshot (snapshot) function, the read operation is not affected by the write operation, can always see consistent data in the reading process.
- In addition, LevelDb also supports data compression to reduce the operation, the storage space and increase IO efficiency have direct help.

### Fast Storage

LevelDb performance is very outstanding, the official website of the random write performance up to 400000 records per second, while the random read performance up to 60000 records per second. In general, the write operation of LevelDb is much faster than the read operation, and the sequential read operation is much faster than random read and write operations. Why is this, see our follow-up to the introduction of LevelDb Gu Yanwu, estimated that you will understand the intrinsic reason.

### Snapshot

In addition, LevelDb supports data snapshot, the read operation is not affected by the write operation, can always see consistent data in the reading process.

On every write, we generate a sequence number. We store the number locally and use the oldest still living snapshot as the oldest version that we have to retain when doing compaction work.

### Recover

![](../architecture/DB/assets/version_recover_10_12.jpeg)

LevelDB will rebuild the lateset version by the leverage of .manifest whenever levelDB starts, which is called recovery.


&nbsp;   
<a id="architecture"></a>
## 5. Architecture

levelDB is NoSQL database, which adopt sequential writing strategy other than random I/O.

![](assets/leveldb_UML_10_01.png)

![](../architecture/SSTable/assets/Minor_Compaction_10_05.png)


&nbsp;   
<a id="module_view"></a>
## 5.1 Module View

![](assets/SA_architecture_10_09.jpg)

As can be seen from the diagram, a LevelDb static structure consists of six main parts: several main file memory in the MemTable and Immutable MemTable and disk: Current file, Manifest file, log file and SSTable file. Of course, LevelDb in addition to the six main parts and some auxiliary files, but these six files and data structure is the main body of the LevelDb elements.

The Log file is consistent with LevelDb and Memtable and Bigtable in the paper., When applied to write a Key:Value record, LevelDb will first go to the log file write, After the success of the record is inserted into the Memtable, So basically completed write operations, For a write operation only involves writing a sequential write once memory and disk, So this is why the main reason LevelDb write fast.

Effect of Log files in the system is mainly used for system crash recovery without loss of data, if there is no Log file, because the written records began is stored in main memory, at this time if the system crashes, the memory of the data have not yet Dump to disk, so the loss of data (Redis problem). To avoid this situation, LevelDb in writing to the memory before the first operation records to the Log file, and then re entered in the memory, so that even if the system crashes, can also restore memory in the Memtable from the Log file, will not cause the loss of data.

When the Memtable is inserted into the data memory to a limit, Need to be exported to the external memory record file, The LevleDb generates a new Log file and Memtable, The original Memtable becomes Immutable Memtable, Seeing the name of a thing one thinks of its function, This means that the contents of Memtable and cannot be changed, Can read but not write or delete. The new coming data is recorded in the new Log file and Memtable, LevelDb background scheduling can export data from a Immutable Memtable to the disk, the formation of a new SSTable file. The formation of SSTable is composed of data in memory has derived and Compaction after operation, and all of the file SSTable is a hierarchical structure, the first layer is the second layer of Level 0, Level 1, and so on, the level gradually increased, which is why called LevelDb causes.

The SSTable file is Key order, That is in the file and key records before Key records, So are all of the Level SSTable, But one thing to note here is: Level 0 SSTable file (..sst) and other Level files compared with special: the levels of.Sst file, Two files may have overlapping key, For example there are two level and 0 SST file, The file A and file B, The file A is the key range: {bar, car}, The file B is the Key range{blue,samecity}, It is very likely that the existence of two documents are key= "blood" record. For other Level SSTable file, does not have the same level of.Sst files within key overlap phenomenon, is that any Level L two.Sst files, you can guarantee that their key values are not overlap. This requires special attention, you will see the difference behind many operations are caused due to this reason.


&nbsp;   
<a id="common_design_models"></a>
## 5.2 Common Design Models

In this section, common designs that are used and standardized in the development of levelDB are described.

### Manifest storage

A file in SSTable belongs to a certain level, and its storage record is key ordered, then there must be a minimum in the key file and key, this is very important information, LevelDb should take note of these information. Manifest is doing, it records the management information of each SSTable file, for example, which belongs to the Level, the name of the file name, what is the minimum key and maximum key. Below is a schematic Manifest storage content:

![](assets/SA_Manifest_10_09.jpg)

### Log File

The main action section about log in the LevelDb file system recovery, to ensure no loss of data. Because before the record to write into the memory of the Memtable, Will be the first to write to the Log file, So even if the system fault, The data in the Memtable before the Dump SSTable file to disk, LevelDB can also be based on the Memtable data structure log file recovery memory, The system will not result in loss of data, LevelDb at this point and Bigtable are identical.

Below we have a look the specific physical and logical layout of the log file is what, LevelDb for a log file, Will cut it down to a physical unit of Block based on 32K, Each reading unit to read a Block as basic unit, The log file which is composed of 3 Block, So from the physical layout of speaking, A log file is composed of continuous 32K size Block.

![](assets/SA_Logfile_10_09.jpg)

### Cache

![](../architecture/util/LRU/assets/handle_table_09_24.png)

![](../architecture/util/LRU/assets/SharededLRUCache_结构_09_24.png)

This is the structure of the LRUCache, which is used in `TableCache` and `BlockCache`.

A read operation if the record is not found in the memory of memtable, to multiple disk access operations. The optimal condition of hypothesis, Is the first in the new level file 0 found in this key, It is also necessary to read the 2 disk, Once the SSTable file in the index part is read into memory, The index can determine the key is stored in a block according to second times is read; the block content, And then look for the key in memory corresponding value.

Two different Cache: Table Cache and Block Cache into levelDb. The Block Cache is configured in the optional, in the configuration file specifies whether to open this function.

#### TableCache

![](assets/SA_TableCache_10_10.jpg)

The Figure is the structure of table cache. In Cache, The value of key is the SSTable file name, Value consists of two parts, One is the file pointer to the disk to open SSTable file, This is for the convenience of reading; another is Table structure pointer to the corresponding memory in the SSTable file, The table structure in memory, Save the index contents of SSTable and cache_id used to indicate block cache, of course, there are also some other content.

For example, in the get (key) read operation, if levelDb key was determined in a level under a file A key range range, so need to judge Is it right? Really contains the k-v file A. At this time, levelDb will first look for Table Cache, see the file is in the cache, if found, then according to the index part can find which block contains the key. If the file is not found in the cache, then open the SSTable file, the index part is read into memory, and then insert the Cache inside, go to index positioning which block contains the Key . If the file which contains the key block, you need to read the block content, this is the second time to read.

#### BlockCache

![](assets/SA_BlockCache_10_10.jpg)

Block Cache is to accelerate this process, the figure is a schematic diagram of the structure. Where key is the file cache_id and the block in the starting position in the file block_offset. Value is the Block content.

If the levelDb finds the block in block cache, you can avoid reading data, block content in cache find key value on the line, if not found? Then read into the block and insert it in block cache. This is the levelDb by two cache to accelerate reading speed. From here we can see that, if the data locality read better, that is to say to read most of the data in cache can be read, then read the efficiency is very high, and if it is on the key sequential reads efficiency should also be good, because once after reading can be repeatedly reused. But if the random read, you can deduce that its efficiency.

### Memtable

![](../architecture/DB/assets/memtable_10_07.jpg)

This section describes the data structure of Memtable memory, an important position in the whole system of Memtable is self-evident. Overall, All k-v data is stored in the Memtable, Immutable Memtable and in SSTable, Immutable Memtable and from the structure of Memtable is exactly the same, The only difference is that it is read-only, Do not allow write operations, While the Memtable is allowed to write and read. When Memtable writes data to a specified number of memory, it is automatically converted to Immutable Memtable, waiting for Dump to the disk, the system will automatically generate a new Memtable for write operation to write new data, understand the Memtable, then Immutable Memtable natural be nothing difficult.

LevelDb MemTable provides k-v data will be written, The operation interface is deleted and read the k-v records, But the fact is that Memtable does not exist in the real delete, delete a Key Value in Memtable is implemented as insert a record, But will hit the delete a Key marker, Really delete operation is Lazy, Will in the future in the process of Compaction to remove the k-v.

Of note, LevelDb Memtable k-v on Key is based on the size of the order in the system memory, insert a new k-v, LevelDb to put the k-v into the right position in order to keep this Key order. In fact, LevelDb Memtable is just an interface class, real operation is done through behind SkipList, including the insertion operation and read operations, so the core data structure of Memtable is a SkipList.

### SSTable

SSTable is a Bigtable critical, the same is true for the LevelDb, the LevelDb SSTable implementation details can also help to understand some of the implementation details in Bigtable.

Static layout structure in this section is mainly about SSTable, the hierarchical structure is formed and we put behind the Compaction section details. This section introduces the SSTable a file in the physical layout and logical structure, the understanding of the operation process of LevelDb very helpful.

![](../architecture/SSTable/assets/leveldb_sstable_structure_10_03.png)

#### Logical Layout

![](assets/SA_SSTable_format_10_09.jpg)

As you can see from this figure, from the large side, .sst files can be divided into data storage and data management, k-v data storage area storing the actual data management areas, provide some index management data, is more rapid and convenient to find the corresponding record. Two regions are in the block on the basis of that file, in front of the plurality of actual k-v data storage, data storage management behind the data management area. Data management is divided into four different types: Purple Meta Block, data index block red MetaBlock index and a blue and a tail of file block.

#### Data Index

![](assets/SA_SSTable_dataindex_10_09.jpg)

This figure is a schematic diagram of the internal structure of data index. Again, Data Block k-v records in the Key arrange the, Each record data index is established on the index information of a Data Block, Each index information includes three contents, Block shown in the figure, the index of index i: the first field red part of the record is greater than or equal to the maximum key data key block i value, The second field points out that the data block I in .sst the starting position in the file, The third field points to Data Block i size (sometimes a data compression).

#### Block data structure

![](../architecture/SSTable/assets/Block_structure_10_02.png)

As can be seen from the graph, its interior is divided into two parts, the front is a k-v record, the order is based on the Key value from small to large, in the Block tail is some "restart" (Restart Point), is actually some pointer, points out that the Block contents in some recording position.

"The restart point "is stem what of? We have repeatedly stressed, The Block content in the k-v record is ordered according to the size of Key, In this way, Two records of adjacent may Key some overlap, For example, key I="the Car", Key i+1="the color",Then there is overlap"the c", In order to reduce the storage capacity of Key, Key i+1 can only store and a different Key part"olor", The common part can be obtained from Key I. Recording of Key content in Block part is so storage, the main purpose is to reduce the storage overhead. "The restart point "the meaning is: at the beginning of this record, no longer take only record the various Key components, but re recording all values of Key, assuming Key i+1 is a restart, then Key will complete storage" the color ", rather than a simple" olor "mode. Block tail is pointed out which records are the restart point.

#### Record format

![](../architecture/SSTable/assets/Block_data_format_10_02.png)

In the Block content area, The internal structure of each k-v record is what? The figure gives the detailed structure, Each record contains 5 fields: key sharing length, For example, the "olor" record, The key and a record shared Key part length is "the C" length, 5; key non shared length, For "olor", 4; value Key:Value Value points out that the length of the length, In the Value content stored in the field behind the actual Value value; while the key non shared content is the actual storage "olor" the Key string.

### Compaction

For LevelDb, the recording operation is very simple, just write a delete delete records label even if finished, but read the record is more complex, need to take in order to find fresh in memory and all levels of file, a very high price. In order to speed up the reading speed, levelDb took compaction to the existing record of finishing compression, by this way, to remove some no longer valid k-v data, reduce the data size, reduce the number of files etc.

The levelDb compaction mechanism and process and Bigtable are basically the same, Bigtable says that three types of compaction:, minor, major and full. The so-called minor Compaction, is the memtable data to an SSTable file; major compaction is associated with different levels of SSTable files, and full compaction is to merge all SSTable.

LevelDb contains two of them, [minor](#minor) and [major](#major).

<a id="minor"></a>
#### Minor Compaction

![](../architecture/SSTable/assets/Minor_Compaction_10_05.png)

As can be seen from the figure, when the number of memtable to a certain extent will be converted to immutable memtable, this time not to write record, only read content from k-v. Introduced before, immutable memtable is in fact a multilevel queue SkipList, the record is based on the key ordered. So the minor compaction implementation is very simple, Is in accordance with the ascending immutable memtable traversal records, And then write a new SSTable file in level 0, The establishment of index data file is finished, So that the completion of a minor compaction. From the figure can be seen, In a deleted record, In the minor compaction process does not actually delete the record, The reason is very simple, Here only know to delete key records, But where is this k-v data? The need of complex search, So when minor compaction does not delete, But will the key as a record to the file, As for the real delete operation, After the higher level of compaction will do.

When the number of a level SSTable file exceeds the set value, levelDb will select a file from the level SSTable (level> 0), the high level of the level+1 SSTable merger documents, this is major compaction.

We know that in more than 0 levels, each SSTable file in Key is to orderly storage, but also between different documents to key range (between the minimum key and maximum key files) will not have any overlap. The Level 0 SSTable file some special, although each file is sorted according to the Key, but since level 0 file is generated directly through minor compaction, so any two level two sstable files under key 0 may have overlapping range. So when major compaction, For more than 0 level level, Choose one of the file on the line, But for level 0, Specify a file, The level is likely to have other SSTable files of the key range and the document overlap, This case, To find all the overlap of the document and level 1 files are merged, Level 0 when the file selection, There may be multiple files in major compaction.

LevelDb compaction in the selection of a level, You also choose any specific files to compaction, LevelDb is a small skills here., That is to say to take turns, For example, the file A compaction, So the next time is in key range is next to the file A file B compaction, So each file will have the opportunity to take turns and high-level level file merge.

<a id="major"></a>
#### Major Compaction

![](../architecture/SSTable/assets/Major_Compaction_10_05.png)

The process of Major compaction are as follows: the multiple files using multi merge sort of way, in order to find out where the minimum Key record, is for all the recording multiple files in the re ranking. After taking a certain standard to judge whether the Key also needs to be saved, if not saved value, then directly away, if you still need to save, then write it to the new generation of level L+1 layer in a SSTable file. So on the one one k-v data processing, formed a series of new L+1 data file, before the L layer file and the L+1 layer in compaction file data at this time has no meaning, so delete all. This completes the combined process of L layer and L+1 layer file record.

Then the major compaction process, determine what is a k-v record is abandoned? Which is a standard: for a key, if less than this Key exists in L layer, the k-v in the major compaction process can drop. Because we the above analyses, the level below the L file if there is the same Key record, so that for Key, there is more fresh Value, so Value in the past is no meaning, so they can be deleted.

### Version

Version saves the file all the information of the current disk and memory, generally only a Version called "version current" (the current version). 

(old)Version + VersionEdit --> (new)Version

VersionEdit will be saved to the MANIFEST file, as data recovery will be from the MANIFEST file to read out data reconstruction.

![](../architecture/DB/assets/Version_VersionEdit_11_08.png)

When a Compaction ends (will generate a new file, before the merger documents need to be deleted), Leveldb creates a new version as the current version, the original version will change into the current version of history.


![](../architecture/DB/assets/VersionSet_11_08.png)

VersionSet is the set of all Version, manages all the survival of Version.
VersionEdit said the changes between Version, equivalent to delta increment, expressed increased the number of files, delete the file. The graph below shows the relationship between them.


&nbsp;   
<a id="evolution_perspective"></a>
## 6. Evolution Perspective

LevelDB is based on concepts from Google's Bigtable database system. The table implementation for the Bigtable system was developed starting in about 2004, and is based on a different Google internal code base than the LevelDB code. That code base relies on a number of Google code libraries that are not themselves open sourced, so directly open sourcing that code would have been difficult. Jeff Dean and Sanjay Ghemawat wanted to create a system resembling the Bigtable tablet stack that had minimal dependencies and would be suitable for open sourcing, and also would be suitable for use in Chrome for the IndexedDB implementation. They wrote LevelDB starting in early 2011, with the same general design as the Bigtable tablet stack, and opened leveldb to the community. levelDB is now still the low-level storage engine behind Google's Bigtable and MapReduce servers.


&nbsp;   
<a id="technical_debt_analysis"></a>
## 7. Technical Debt Analysis

### Defect Debt

LevelDB is used in bitcoin and it had a bug that, under rare conditions, could cause it to consistently return not found on records that were really there. . . Leveldb fixed this serious bug in a minor update. But deploying a fix like this in an uncontrolled manner in the bitcoin network would potentially cause a fork in the consensus state; so any such fix would need to be rolled out in an orderly manner.

### Codestyle Debt

- Mush use of type erasure impedes the progress of reading source code and maintain the project.
- I've once noticed that the iteration_expression in for-loop is located at the end of the code block, which is more than 100 lines below the for-loop.

### Code Quality

[![CodeFactor](https://www.codefactor.io/repository/github/rsy56640/leveldb/badge)](https://www.codefactor.io/repository/github/rsy56640/leveldb)

Some files quality detection:

![](assets/SA_code_quality_11_18.png)

In fact, it's absolutely more than B+, though many functions and modules in levelDB is sort of large. It is of great necessity for a storage engine to do some complex work.


&nbsp;   
<a id="conclusion"></a>
## 8. Conclusion

LevelDB is designed to be used as persistent storage engine in Google Bigtable. It provides users with fast k-v storage and snapshot, but without server and client backend. It has been widely used in database domain, such as bitcoin storage engine. Facebook has developed [RocksDB](https://github.com/facebook/rocksdb) based on levelDB. In addition, levelDB made a good start for distributed storage.


&nbsp;   
<a id="references"></a>
## 9. References

- [What are the keys used in the blockchain levelDB (ie what are the key:value pairs)?](https://bitcoin.stackexchange.com/questions/28168/what-are-the-keys-used-in-the-blockchain-leveldb-ie-what-are-the-keyvalue-pair)
- [Node.js + LevelDB](https://coderead.wordpress.com/2013/04/04/node-js-leveldb/)
- [The Google forum for levelDB](https://groups.google.com/forum/#!forum/leveldb)   
- [Issues of levelDB on Github](https://github.com/google/leveldb/issues)