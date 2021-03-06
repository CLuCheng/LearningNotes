# [学习笔记] Java核心技术 卷二：Java SE 8 的流库（一）

- 流和集合的差别：
  * 流不存储元素
  * 流的操作不修改数据源
  * 流的操作是尽可能地惰性执行
- 操作流的流程：
  * 创建一个流
  * 指定将初始流转化为其他流的中间操作
  * 终止操作，产生结果
- stream()  顺序流
- parallelstream()  并行流
- Stream.of数据转化为流
- Arrary.stream(array, from, to)可以从from和to的元素中创建一个流
- filter 产生一个流，包含当前流中所有满足断言条件的元素
- map产生一个流，包含将mapper应用于当前流中所有元素产生的结果
- flatMap产生一个流，包含将mapper应用于当前流中所有元素产生的结果连接到一起而获得的
- limit(n)产生一个流，包含了流中最初的n个元素
- skip(n)产生一个流，丢弃了流中最初的n个元素
- concat 连接2个流
- distinct 产生一个流，包含所所有不同元素
- sorted 排序
- peek 产生一个流，获取一次数据，调用一个函数
- **约简**一种终结操作，将流约简为可以在程序中使用的非流值
- max最大，min最小，findFirst产生第一个元素findAny产生任意一个元素
- anyMatch，allMatch，noneMatch，分别在这个流中任意元素，所有元素，没有任何元素匹配给定断言是返回true
- Optional < T > 对象是一种包装器对象，要么包装某个对象，要么没有包装任何对象
- 遍历流 iterator 或者 forEach 
- forEachOrdered按照流的顺序处理并行流
- toArray转化数组
- collect 给指定收集器收集当前流中的元素
- groupingBy 方法会产生一个映射表，每个值都是一个列表。如果要处理这些列表，需提供“下游收集器”
- parallel方法可以将任意的顺序流转换为并行流
- unordered产生一个无序流
- 让并行流正常工作，需要满足的条件：
  * 数据应在内存中
  * 流应该可以被高效地分为若干个子部分
  * 流材质的工作量应该具有较大的规模
  * 流操作不应该被阻塞