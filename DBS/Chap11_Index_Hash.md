# Basic

* Why we need an index?

* Indexing mechanisms used to speed up access to desired data.

    * E.g., author catalog in library

* Search Key - attribute or set of attributes used to look up records in a file.

* An index file consists of records (called index entries) of the form:

    |            |         |
    | ---------- | ------- |
    | Search-Key | Pointer |

* Index files are typically much smaller than the original file.

* Two basic kinds of indices: (index文件中索引记录如何组织？取决于索引类型)

    * Ordered indices (顺序索引):  search keys (index entries) are stored in sorted order
    * Hash indices (散列索引):  search keys (index entries) are distributed uniformly across “buckets” using a “hash function”. 

## Evaluation Metrics(度量指标)

* Access types which are supported efficiently (能有效支持的存取类型).  E.g., 
    * records with a specified value in the attribute. 
        * where Col=v
    * or records with an attribute value falling in a specified range of values.  
        * E.g. where Col Between v1 and v2 . 
    * <u>Hash index is not good for ‘Between’ query condition, but ordered index is good.</u>
* Access time
* Insertion time
* Deletion time
* Space overhead (空间开销)
* 时间效率和空间效率是衡量索引技术最主要的指标，也是数据库系统组织和管理技术的关注的焦点之一。

## Ordered Indices

**基本概念**

* In an ordered index, index entries are stored sorted on the search key value. E.g., author catalog in library.
* Sequentially ordered file (顺序排序文件): The records in the file (data file) are ordered by a search-key.   (Chapter 10)
* Primary index: is an index whose search key equal to the search key of the sequentially ordered data file, on which the index is made. (与对应的数据文件本身的排列顺序相同的索引称为主索引。)
    * Also called clustering index (聚集索引)
    * The search key of a primary index is usually but not necessarily the primary key. 主索引的搜索键通常是但并非一定是主码。
        * Non-sequential files don’t have primary index, but the relations can have primary key.
    * Index-sequential file (索引顺序文件): sequentially ordered file with a primary index.
* Secondary index (辅助索引): an index whose search key specifies an order different from the sequential order of the file. Also called non-clustering index. 与本来的不一样的索引，另一种索引方式？
* Two types of ordered indices: 
    * dense index (稠密索引)
    * sparse index (稀疏索引)

### Dense Index File

File中每个索引值都在有索引文件中有索引项

？？？？？？

### Sparse Index File

？

* Sparse Index:  contains index entries for only some search-key values. (Usually, one block of data gives an index entry, a block contains a number of ordered data records)
    * Applicable only when data file records are sequentially ordered on search-key
* To locate a record with search-key value K , (搜索方法)
    * Step1: Find index record with largest search-key value < K。 E.g. for 33456 -> 32343 (See next page)
    * Step2: Search file sequentially starting at the record to which the index entry points

