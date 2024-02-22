# Commands

Table of Contents
- Topics
- Producer
- Consumer
- Consumer Groups
- KRaft

## Topics
### To create a new topic
```yaml {.code-highlight}
kafka-topics.sh --create --topic <TOPIC_NAME> --bootstrap-server localhost:9092
```
By default the partition is set to 1 and the replication factor is set to 1. You can specify certain partition and replication factor by
```yaml {.code-highlight}
kafka-topics.sh --create --topic <TOPIC_NAME> --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
```
To list the topics
```yaml {.code-highlight}
kafka-topics.sh --list --bootstrap-server localhost:9092
```
To describe a topic
```yaml {.code-highlight}
kafka-topics.sh --describe --topic <TOPIC_NAME> --bootstrap-server localhost:9092
```
To delete a topic
```
kafka-topics.sh --delete --topic <TOPIC_NAME> --bootstrap-server localhost:9092
```

## Producer
### To produce messages to the topic
```
kafka-console-producer.sh --topic <TOPIC_NAME> --bootstrap-server localhost:9092
```
NOTE: By default if there is no topic exist as you specified then Kafka will create that topic with the default partitions and replication factors mentioned in the server.properties You can edit the default configurations at kafka/config/server.properties

We can set different Acknowledgement level by
```
kafka-console-producer.sh --topic <TOPIC_NAME> --bootstrap-server localhost:9092 --producer-property acks=all
```
## Consumer
### To consume the topic
```
kafka-console-consumer.sh --topic <TOPIC_NAME> --bootstrap-server localhost:9092
```
By default it will start consuming the message after the above command executed. If need to consume from the beginning

kafka-console-consumer.sh --topic <TOPIC_NAME> --bootstrap-server localhost:9092 --from-beginning
To have a consumer inside a group

kafka-console-consumer.sh --topic <TOPIC_NAME> --bootstrap-server localhost:9092 --group log-application-group-1
Consumer Groups
To list all the consumer groups

kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
To describe a consumer group

kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group <CONSUMER_GROUP>
By this command we can check the current offset of the consumer group, the lag and the log offset.

To reset the offset of a consumer group

kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group <CONSUMER_GROUP> --reset-offsets --to-earliest --execute --topic first_topic
This command will reset the offset for a specific topic. To reset for all topics

kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group <CONSUMER_GROUP> --reset-offsets --to-earliest --execute --all-topics
To go before 2 offset

kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group <CONSUMER_GROUP> --reset-offsets --shift-by -2 --execute --all-topics
If provided positive number it will shift forward and a negative number will shift backward

To delete a consumer group

kafka-consumer-groups.sh --bootstrap-server localhost:9092 --delete --group <CONSUMER_GROUP>
NOTE: You can’t delete a consumer group when it is active

KRaft
To describe the KRaft meta.properties

kafka-metadata-quorum.sh --bootstrap-server localhost:9092 describe --status
To dump the log file for debugging

kafka-dump-log.sh --cluster-metadata-decoder --files <PATH_TO_LOG>
