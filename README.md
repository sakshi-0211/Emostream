# EmoStream - Kafka and Spark Streaming Project

## Project Overview
EmoStream is a real-time emoji streaming system using Apache Kafka and Spark for processing and aggregating emoji data. This project consists of multiple publishers, a dynamic client manager, and a Spark-based aggregation pipeline.

## Prerequisites
Ensure you have the following dependencies installed:
- Apache Kafka
- Apache Spark
- Conda (for virtual environment management)
- Python 3
- Locust (for load testing)
- Pytest (for unit testing)

## Commands to Run the Project

### 1. Start Zookeeper and Kafka Server
```bash
cd /etc/systemd/system/
sudo systemctl stop zookeeper

cd /usr/local/kafka
sudo bin/kafka-server-start.sh config/server.properties
sudo bin/zookeeper-server-start.sh config/zookeeper.properties
```

### 2. Create Kafka Topics
```bash
cd /usr/local/kafka
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic emoji_topic --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic emoji_topic_aggregated --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic cluster_topic_1 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic cluster_topic_2 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic cluster_topic_3 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic subscriber_topic_1 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic subscriber_topic_2 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic subscriber_topic_3 --partitions 3 --replication-factor 1
```

### 3. Start Spark Streaming Job
```bash
conda activate bd
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.0 emoji_aggregator_new.py
```

### 4. Start Backend Services
```bash
conda activate bd
python3 app_new.py
python3 main_publisher.py
python3 cluster_publisher_1.py
python3 cluster_publisher_2.py
python3 cluster_publisher_3.py
python3 dynamic_client_manager.py
python3 aggregation_viewer.py
python3 emo_stream_server.py
```

### 5. Access Frontend
Visit:
```
http://localhost:5001
```

### 6. Run Tests
#### Unit Tests
```bash
python -m pytest test_emoji.py -v
```

#### Load Testing
```bash
locust -f test_emoji.py --host=http://localhost:5000 --users 100 --spawn-rate 10
```

## Notes
- Ensure all required services are up and running before executing the scripts.
- Modify configurations as needed based on your environment.

## Contributors
- **Sakshi Rajani**
- **Samarth P**
- **Sadhana Shashidhar**
- **Sakshi Masand**

