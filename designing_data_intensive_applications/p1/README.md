# 第一部分 数据系统基础

开始的四个章节讲述了基础理念，无论是单机还是在分布式集群的数据系统，这些理念都有所应用：

1. [第一章](c1/README.md)介绍了常用术语与方法，例如，我们在提到*可靠性*，*可扩展性*，*可维护性*的时候，我们通常想表达什么，以及怎么达成
目标。

2. [第二章](c2/README.md)从开发人员的视角介绍了两个最重要的因素——数据模型与查询语句。我们将会展现特定的模型如何被应用于特定的场景之中。

3. [第三章](c3/README.md)将深入存储引擎的内部，挖掘数据库怎么组织磁盘中的数据。究其原因，不同的存储引擎都是为了特定场景中的负载进行了优
化，而基于这个了解选取合适的引擎会对整体系统的性能有巨大的影响。

4. [第四章](c4/README.md)比较了多种数据编码（序列化）的格式，并探究了他们在应用需求和数据模式快速变更的场景下的适应性。

随后，[第二部分](../p2/README.md)会谈到分布式数据系统的特定问题。
