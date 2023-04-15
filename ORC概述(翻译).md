# 背景
在2013年1月以前,为了大规模加速Apache Hive和提升Apache Hadoop上数据存储效率.重点是高速的数据处理以及减少文件大小

ORC是由Hadoop workloads设计的自描述的列式文件格式.它针对大型流式读取进行了优化,但同时也具备快速查找所需行功能.列式存储的数据仅需
读、解压和处理那些需要查询的列.因为ORC文件时类型识别的,writer会为每种数据类型选择最合适的编码算法,并且会在写文件时为数据构建内部
索引.

使用数据索引(以下简称Indices)的谓词下推(Predicate PushDown,以下简称PPD)可以确定文件中哪些Stripe是需要被读入的,indices能够缩小
查询到一个特定的数据片段(大小可配置,默认是10000行).ORC完全支持Hive的数据类型, 包括复杂类型:struct,list,map,和union.

需要大型Hadoop用户已经采用ORC了.例如,Facebook使用ORC保存[TB级别数据在他们数据仓库](https://s.apache.org/fb-scaling-300-pb),并且声明ORC相比RC和Parquet是明显更快.Yahoo使
用ORC保存他们生产数据,并且公布了他们的[benchmark结果](https://s.apache.org/yahoo-orc).

ORC文件被切分成多个被称为Stripe的数据片,Stripe大小可配置,默认为64M.Stripe在文件中是互相独立的,这样方便对一个文件并行读取.在每个
Stripe内, 每列数据也是独立存储,这样很方便Reader按需读取关注的列.

ORC格式更多特性,请关注[ORC格式定义](ORC%20V2格式定义.md)
