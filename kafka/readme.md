可以看到这些消息被相对均匀的分布到0和1这2个分区里面。另外可以在其他终端上用别的GroupID订阅消息如
```
go run sarama_consumer.go localhost:9092 2 sarama
```
(GroupID为2)。

如果多个终端的GroupID一样，不同的进程会消费绑定的对应分区里面的消息，不会重复消费。举个例子：有2个终端执行

```
go run sarama_consumer.go localhost:9092 1 sarama
```
，那么终端A会消费0分区的，终端B会消费1分区的消息。但有3个终端执行的话，由于目前只有2个分区，终端C由于没有绑定到分区，什么都不会消费


```
❯ go run confluent_producer.go localhost:9092 strconv
```
Created Producer rdkafka#producer-1
Delivered message to topic strconv [0] at offset 0
```
❯ go run confluent_producer.go localhost:9092 strconv
```
Created Producer rdkafka#producer-1
Delivered message to topic strconv [0] at offset 1
