## DeepLog:  Anomaly Detection and Diagnosis from System Logs through Deep Learning

> CCS 2017
>
> Min Du, Feifei Li, Guineng Zheng, Vivek Srikumar School of Computing, University of Utah

论文主要提出`DeepLog`，基于 `Long Short-Term Memory(LSTM)` 的神经网络模型，将系统日志建模为自然语言序列，进而完成异常检测任务。

### ABSTRACT

1. 系统日志的作用。（为什么要分析系统日志）
   * 系统日志用来记录系统的状态和重要事件，从而帮助分析系统错误和用户导致的异常表现。
   * 系统日志是在线监测和异常检测最好的信息资源。
2. 介绍 `DeepLog` 的核心方法与功能。（为什么选择DeepLog）
   * 它可以自动从普通的日志输出中生成日志模型并进行异常检测。
   * 它可以增量式地更新DeepLog模型，所以随着时间的推移它可以适应新的日志系统。
   * 它可以根据系统日志构建工作流，因此一旦异常被检测到，用户就可以快速诊断检测到的异常和用户引起的异常表现。

### INTRODUCTION

1. 在搭建安全可靠的计算机系统的时候，异常检测尤为重要。

2. 异常检测面临着诸多挑战，很多以前基于挖矿方法论的异常检测算法已经不再有效了。

3. 系统日志的作用。

   *	系统日志用来记录系统的状态和重要事件，从而帮助分析系统错误和用户导致的异常表现。
   *	系统日志是在线监测和异常检测最好的信息资源。

4. 现有的三类算法：

   * 基于PCA的日志消息计数器方法。
   * 基于不变挖掘的方法来捕获不同日志键之间的共现模式。
   * 基于工作流的程序逻辑流执行异常识别方法。

   虽然这些算法在某些情况下是成功的，但是它们都不能普遍应用于异常检测。

5. 简单介绍 `DeepLog` ：

   * 是一种基于大量系统日志的数据驱动算法。
   * 主要灵感来源于自然语言处理：将日志条目视为遵循某些样式和语法规则的序列的元素。
   * 是一个深度神经网络，可以对日志序列进行建模使用长期短期记忆（LSTM）的条目。
   * 可以动态地更新模型，因此可以适应新的日志系统。
   
6. 挑战：

   * 日志数据是非结构化数据，它的格式和语义在不同系统间可能差异很大。一些现有的方法使用基于规则的方法来解决这个问题，但是规则的设计依赖于领域知识，例如工业界常用的正则表达式。 更重要的是，基于规则的方法对于通用异常检测来说并不适用，因为我们几乎不可能预先知道不同类型日志中的关注点是什么。 
   * 时效性。为了用户能够及时的发现系统出现的异常，异常检测必须要及时，日志数据以数据流的形式输入，意味着需要对整个日志数据做分析的方法不适用。 
   * 日志消息由多个不同的线程或正在运行的任务产生。这种并发性使得我们难以应用基于工作流的异常检测方法，该方法将针对单个任务的工作流模型用作生成模型来与一系列日志消息进行匹配。
   * 异常类型多。系统和应用程序可能会产生多种类型的异常。我们希望异常检测系统不仅针对于特定的异常类型，还能检测未知的异常。同时，日志消息中也包含了丰富的信息，比如log key、参数值、时间戳等。大多数现有的异常检测方法仅仅分析了日志消息的特定部分(比如log key)，这限制了它们能检测到的异常类型。 

7.   相关工作：

   1. DeepLog不仅在日志条目中使用日志key，还使用度量值进行异常检测，因此，它能够捕获各种类型的异常。DeepLog仅依赖于一个小的训练集，该数据集由一系列“正常日志条目”组成。在训练阶段，DeepLog可以识别正常的日志序列，并可以以流方式用于对传入日志条目进行在线异常检测。
   2. 直观上，DeepLog隐式地从与常规系统执行路径相对应的训练数据中捕获日志条目之间潜在的非线性和高维依赖性。为了帮助用户在发现异常后诊断问题，DeepLog还在其培训阶段根据日志条目构建工作流模型。DeepLog将并发任务或线程生成的日志项分成不同的序列，以便为每个单独的任务构建工作流模型。
   3. 我们的评估结果显示，在先前工作所探索的大型HDFS日志数据集上，仅训练了与正常系统执行相对应的非常小部分（不到1%）的日志条目，DeepLog可以在剩余99%的日志条目上实现几乎100%的检测精度。大型OpenStack日志的结果传达了类似的趋势。此外，DeepLog还提供了通过合并实时用户反馈在检测阶段增量更新其权重的功能。更具体地说，如果一个正常的日志条目被错误地归类为异常，DeepLog提供了一种用户反馈机制。



