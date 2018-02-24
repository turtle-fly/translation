第一章 可靠的，可扩展的，可维护的应用
=================================

> 互联网构建的如此好用，以至于人们把它当成了像水一般触手可及的自然资源，而忘记它其实也是人工搭建起来的。我们甚至都无法想起有哪一项工程技术有
> 如此庞大的规模但同时又有极好的容错性。
>     —— Alan Kay，在接受 Dr Dobb 日报采访时如是说

相对于*计算密集型*的任务，现在许多的应用都是*数据密集型*的，CPU的运算能力并非这些应用增长的瓶颈。关键的问题在于数据的规模、复杂性以及高频
率的变更。

一个数据密集型的应用往往是在一些通用功能的组件基础上构建起来的，这些应用可以：

- 存储数据，以供自己或者其他数据随后访问（数据库）
- 将一个开销很大的计算记过保存下来，以提升读的效率（缓存）
- 允许用户检索和过滤数据（索引）
- 将消息发送给另一个进程，以提供异步处理的能力（流处理）
- 周期性的对累积到很大规模的数据进行处理（批处理）
*原文crunch有拿着压缩饼干并用牙齿大声咀嚼的画面感……*

看到这些日常使用的功能，你会想这太明显不过了！那是因为这些*数据系统*都已经做了成功的抽象，我们不用多想就可以简单的使用。在构建一个应用的时
候，大多数工程师都不用去考虑从零开始写一个新的数据存储引擎，因为数据库已经完美的帮你实现了这些功能。

但事实并没有那么简单。每个不同的数据库系统都有各自的特性，他们本就诞生自不同的需求。做缓存有很多种方法，做索引亦是如此。当我们构建一个应用
的时候，我们仍然需要思考哪些工具和方法对手头的工作是最合适的。并且在将多个工具组合起来去实现一个目标的时候，我们更需要仔细的思考。

这本书将带你从原理和实践方面解读上面提到的那些数据系统，并讲解怎样用这些数据系统来搭建一个数据密集型的应用。我们将会探讨不同工具间的共性、
特点、以及他们的实现方式。

为了实现可靠地、可扩展的、可维护的数据系统，这章我们将从基础开始探索。我们将会阐述这些概念的定义，勾勒对这些概念进行思考的方式，给随后章节
打下基础。在随后的章节中，我们将会对数据密集型应用中每一层的设计决策进行思考。


#引用
<a id="b1_c1_1"></a>
[1] Michael Stonebraker and Uğur Çetintemel: 
“[**‘One Size Fits All’: An Idea Whose Time Has Come and Gone**](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.68.9136&rep=rep1&type=pdf),” 
at *21st International Conference on Data Engineering* (ICDE), April 2005.

<a id="b1_c1_2"></a>
[2] Walter L. Heimerdinger and Charles B. Weinstock: 
“[**A Conceptual Framework for System Fault Tolerance**](https://www.sei.cmu.edu/reports/92tr033.pdf),” 
Technical Report CMU/SEI-92-TR-033, Software Engineering Institute, Carnegie Mellon University, October 1992.

<a id="b1_c1_3"></a>
[3] Ding Yuan, Yu Luo, Xin Zhuang, et al.: 
“[**Simple Testing Can Prevent Most Criti‐ cal Failures: An Analysis of Production Failures in Distributed Data-Intensive Sys‐ tems**](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf),” 
at *11th USENIX Symposium on Operating Systems Design and Implementation* (OSDI), October 2014.

<a id="b1_c1_4"></a>
[4] Yury Izrailevsky and Ariel Tseitlin: 
“[**The Netflix Simian Army**](http://techblog.netflix.com/2011/07/netflix-simian-army.html),” 
*techblog.netflix.com*, July 19, 2011.

<a id="b1_c1_5"></a>
[5] Daniel Ford, François Labelle, Florentina I. Popovici, et al.: 
“[**Availability in Glob‐ ally Distributed Storage Systems**](https://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/36737.pdf),” 
at *9th USENIX Symposium on Operating Systems Design and Implementation* (OSDI), October 2010.

<a id="b1_c1_6"></a>
[6] Brian Beach: 
“[**Hard Drive Reliability Update – Sep 2014**](https://www.backblaze.com/blog/hard-drive-reliability-update-september-2014/),” 
*backblaze.com*, September 23, 2014.

<a id="b1_c1_7"></a>
[7] Laurie Voss: 
“[**AWS: The Good, the Bad and the Ugly**](https://web.archive.org/web/20160429075023/http://blog.awe.sm/2012/12/18/aws-the-good-the-bad-and-the-ugly/),” 
*blog.awe.sm*, December 18, 2012.

<a id="b1_c1_8"></a>
[8] Haryadi S. Gunawi, Mingzhe Hao, Tanakorn Leesatapornwongsa, et al.: 
“[**What Bugs Live in the Cloud?**](http://ucare.cs.uchicago.edu/pdf/socc14-cbs.pdf),” 
at *5th ACM Symposium on Cloud Computing* (SoCC), November 2014. [**doi:10.1145/2670979.2670986**](http://dx.doi.org/10.1145/2670979.2670986)

<a id="b1_c1_9"></a>
[9] Nelson Minar: 
“[**Leap Second Crashes Half the Internet**](http://www.somebits.com/weblog/tech/bad/leap-second-2012.html),” 
*somebits.com*, July 3, 2012.

<a id="b1_c1_10"></a>
[10] Amazon Web Services: 
“[**Summary of the Amazon EC2 and Amazon RDS Ser‐ vice Disruption in the US East Region**](https://aws.amazon.com/cn/message/65648/),” 
*aws.amazon.com*, April 29, 2011.

<a id="b1_c1_11"></a>
[11] Richard I. Cook: 
“[**How Complex Systems Fail**](http://web.mit.edu/2.75/resources/random/How%20Complex%20Systems%20Fail.pdf),” 
Cognitive Technologies Laboratory, April 2000.

<a id="b1_c1_12"></a>
[12] Jay Kreps: 
“[**Getting Real About Distributed System Reliability**](http://blog.empathybox.com/post/19574936361/getting-real-about-distributed-system-reliability),” 
*blog.empathybox.com*, March 19, 2012.

<a id="b1_c1_13"></a>
[13] David Oppenheimer, Archana Ganapathi, and David A. Patterson: 
“[**Why Do Internet Services Fail, and What Can Be Done About It?**](http://static.usenix.org/legacy/events/usits03/tech/full_papers/oppenheimer/oppenheimer.pdf),” 
at *4th USENIX Symposium on Internet Technologies and Systems* (USITS), March 2003.

<a id="b1_c1_14"></a>
[14] Nathan Marz: 
“[**Principles of Software Engineering, Part 1**](http://nathanmarz.com/blog/principles-of-software-engineering-part-1.html),” 
*nathanmarz.com*, April 2, 2013.

<a id="b1_c1_15"></a>
[15] Michael Jurewitz: 
“[**The Human Impact of Bugs**](http://jury.me/blog/2013/3/14/the-human-impact-of-bugs),” 
*jury.me*, March 15, 2013.
