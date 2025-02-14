Commands to run the project:

cd /etc/systemd/system/
sudo systemctl stop zookeeper

cd /usr/local/kafka
sudo bin/kafka-server-start.sh config/server.properties
sudo bin/zookeeper-server-start.sh config/zookeeper.properties

cd /usr/local/kafka
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic emoji_topic --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic emoji_topic_aggregated --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic cluster_topic_1 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic cluster_topic_2 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic cluster_topic_3 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic subscriber_topic_1 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic subscriber_topic_2 --partitions 3 --replication-factor 1
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --topic subscriber_topic_3 --partitions 3 --replication-factor 1

conda activate bd
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.0 emoji_aggregator_new.py

conda activate bd
python3 app_new.py 
python3  main_publisher.py
python3  cluster_publisher_1.py
python3  cluster_publisher_2.py
python3  cluster_publisher_3.py
python3 dynamic_client_manager.py
python3 aggregation_viewer.py
python3 emo_stream_server.py
---> frontend: localhost:5001

python -m pytest test_emoji.py -v
locust -f test_emoji.py --host=http://localhost:5000 --users 100 --spawn-rate 10
