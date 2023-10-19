# Kafka Setup

## Overview
This guide outlines the steps to set up Kafka, Zookeeper, and Connect. The setup involves running Debezium's connectors to turn existing databases into event streams.

## Steps
1. **Set up Zookeeper**: Run a Zookeeper container.
    ```
    docker run -dit --name zookeeper -p 2181:2181 -p 2888:2888 -p 3888:3888 debezium/zookeeper:1.6
    ```

2. **Create Kafka Container**: Run a Kafka container.
    ```
    docker run -dit --name kafka -p 9092:9092 --link zookeeper:zookeeper debezium/kafka:1.6
    ```

3. **Set up Debezium Connect**: Run a Debezium connect container.
    ```
    docker run -dit --name connect -p 8083:8083 -e GROUP_ID=1 -e CONFIG_STORAGE_TOPIC=my-connect-configs -e OFFSET_STORAGE_TOPIC=my-connect-offsets -e STATUS_STORAGE_TOPIC=my_connect_statuses --link zookeeper:zookeeper --link kafka:kafka --link mysql:mysql debezium/connect:1.6
    ```

4. **Test Kafka Connect**: Verify if Kafka Connect is up and running.
    ```
    curl -H "Accept:application/json" localhost:8083
    ```

5. **Enable MySQL Debezium Connector**: Run the following command to enable the MySQL Debezium connector for Kafka Connect.
    ```
    curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" localhost:8083/connectors/ -d '{ "name": "inventory-connector", "config": { "connector.class": "io.debezium.connector.mysql.MySqlConnector", "tasks.max": "1", "database.hostname": "mysql", "database.port": "3306", "database.user": "debezium", "database.password": "dbz", "database.server.id": "184054", "database.server.name": "dbserver1", "database.include.list": "demo", "database.history.kafka.bootstrap.servers": "kafka:9092", "database.history.kafka.topic": "dbhistory.demo" } }'
    ```

6. **Check Kafka Topic**: Verify if the Kafka topic was created.
    ```
    docker exec -it kafka bash
    bin/kafka-topics.sh --list --zookeeper zookeeper:2181
    ```

7. **Monitor Kafka Topic**: Check if the Kafka topic is receiving data.
    ```
    bin/kafka-console-consumer.sh --topic dbserver1.demo.bus_status --bootstrap-server '<container_id>':9092
    ```
