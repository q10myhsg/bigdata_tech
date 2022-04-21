
#  Flink exactly once 
Flink与外界系统交互，经常需要保证幂等性，也就是需要保exactly once. 一般这里需要配合checkpoint机制保证flink系统的exactly once。
## 前置条件
* flink 本身开启checkpoint，数据恢复时会从固定的checkpoint恢复，而且有指定的后端存储
* source
    * 必须支持从指定的为位置消费，典型应用就是kafka
* sink
    * 下游也必须支持事务写入机制，也就是能配合flink的两阶段提交机制

    
 # checkpoint
 
 checkpoint 就是检查点的意思，是flink实现容错机制的核心功能，是flink可靠性的基石。flink的checkpoint相当于是数据流中标记位置，flink会在数据流中插入一条标识标记的记录叫做checkpoint barrier.
 
##常用参数
    
    checkpoint 周期
    
    
    
    ## 常用存储原理
    
## checkpoint工作原理·
涉及几个关键的组件：

* checkpoint Coordinator  协调者，一般由jobmanager来承担
* source task：checkpoint barrier插入的源头
* sink task: checkpoint 结束标志

工作步骤：
1. checkpoint coordinator 向sink task发送checkpoint trigger
2. sink向 数据流中插入check barrier
3. barrier流入其他算子
 * 出现合流的时候回因为barrier对齐，也就是当一个算子数据由多个流汇集而成，会等待所有barrier到达后再向下游传递，期间数据会放到缓冲区中。 这也是背压问题产生的原因。
 * barrier流出算子会jobmanager 汇报
 * 当sink算子汇报jobmanager后 checkpoint本次checkpoint结束


    
    # 两阶段提交
    ## 两阶段提交原理
    
    
    ## kafka两阶段提交
    
    
    ## mysql 两阶段提交
    
    
    ## 曾经遇到的坑
    
    
    



