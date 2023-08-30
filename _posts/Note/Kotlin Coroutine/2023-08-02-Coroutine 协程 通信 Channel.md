---
layout: post
tags: Android Coroutine
---

# 协程通信 Channel

- 简单使用

```
suspend fun main(){
    val channel = Channel<Int>()
 
    val producer = GlobalScope.launch {
        var i = 0
        while (true){
            delay(100)
            channel.send(i++)
        }
    }
 
    val consumer = GlobalScope.launch {
        while (true){
            println(channel.receive())
        }
    }
 
    producer.join()
    consumer.join()
}
```

- channel迭代

iterator.hasNext()是挂起函数，在判断是否有下一个元素的时候就需要去Channel中读取元素了

```
suspend fun main() {
    val channel = Channel<Int>()
 
    val producer = GlobalScope.launch {
        var i = 0
        while (i <= 5) {
            delay(100)
 
            println("send before")
            channel.send(i++)
            println("send after")
 
        }
        //关闭通道
        channel.close()
    }
 
    val consumer = GlobalScope.launch {
        val iterator = channel.iterator()
        while (iterator.hasNext()) {
            println(iterator.next())
        }
 
        println("exit consumer")
    }
 
    producer.join()
    consumer.join()
}
```

以上为 一对一, 数据仅可被单次读取

生产-消费 API, 观察者模式/订阅者模式

- BroadcastChannel, 广播通道

发送端和接收端在 Channel 中存在一对多的情形, 可以同时被多个 消费者协程 接收

```
runBlocking {
    // BroadcastChannel 广播通道, 传递 Int 类型数据
    val broadCastChannel = BroadcastChannel<Int>(Channel.BUFFERED)

    // 数据生产者协程
    val producer = GlobalScope.launch {
        for (i in 0..2) {
            delay(100)
            broadCastChannel.send(i)
            println("向通道中发送数据 $i")
        }
        // 关闭通道
        broadCastChannel.close()
    }
    
    List(3) { index ->
        // 数据消费者协程
        GlobalScope.launch {
            val channel = broadCastChannel.openSubscription()
            for (num in channel) {
                delay(1000)
                println("消费者协程 $index 从通道中接收数据 $num")
            }
        }
    }.joinAll()
}
```
