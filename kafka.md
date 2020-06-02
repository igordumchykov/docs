### Kafka commands

1. Consume from Kafka
  ```bash
  kubectl exec -it kafka-0 -- kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic foriscallback-timestamp-to-retry-event-store-changelog --from-beginning --property print.key=true
  ```
5. Remove messages from topic
  ```bash
  kubectl exec -it kafka-0 -- kafka-configs.sh --zookeeper kafka-zookeeper:2181 --entity-type topics --alter --entity-name foriscallback-timestamp-to-retry-event-store-changelog --add-config retention.ms=1000
  kubectl exec -it kafka-0 -- kafka-configs.sh --zookeeper kafka-zookeeper:2181 --entity-type topics --alter --entity-name foriscallback-timestamp-to-retry-event-store-changelog --add-config segment.ms=1000
  kubectl exec -it kafka-0 -- kafka-configs.sh --zookeeper kafka-zookeeper:2181 --entity-type topics --alter --entity-name foriscallback-timestamp-to-retry-event-store-changelog --add-config cleanup.policy=delete
  kubectl exec -it kafka-0 -- kafka-configs.sh --zookeeper kafka-zookeeper:2181 --entity-type topics --alter --entity-name foriscallback-timestamp-to-retry-event-store-changelog --delete-config retention.ms
  kubectl exec -it kafka-0 -- kafka-configs.sh --zookeeper kafka-zookeeper:2181 --entity-type topics --alter --entity-name foriscallback-timestamp-to-retry-event-store-changelog --delete-config segment.ms
  kubectl exec -it kafka-0 -- kafka-configs.sh --zookeeper kafka-zookeeper:2181 --entity-type topics --alter --entity-name foriscallback-timestamp-to-retry-event-store-changelog --add-config cleanup.policy=compact
  ```
  retention.ms=604800000 - default value
  6. Get topic config
  ```bash
  kubectl exec -it kafka-0 -- kafka-configs.sh --zookeeper kafka-zookeeper:2181 --entity-type topics --entity-name foriscallback-timestamp-to-retry-event-store-changelog --describe
  ```
  7. Send message with key
  ```bash
  kubectl exec -it kafka-0 -- kafka-console-producer.sh  --broker-list localhost:9092 --topic foriscallback-timestamp-to-retry-event-store-changelog --property "parse.key=true" --property "key.separator=|"
  key|value
  ```
