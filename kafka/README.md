# Creating a User for Kafka
```bash
useradd kafka -m
passwd kafka
usermod -aG wheel kafka
usermod -aG wheel kafka
```

# Downloading and Extracting the Kafka Binaries
```bash
wget https://www.apache.org/dist/kafka/3.2.0/kafka_2.12-3.2.0.tgz
mkdir ~/kafka && cd ~/kafka
tar -xvzf ~/Downloads/kafka.tgz --strip 1
```
# Configuring the Kafka Server
```bash
vi ~/kafka/config/server.properties
```
add the following line at the end of the file
```bash
delete.topic.enable = true
```

# Creating Systemd Unit Files and Starting the Kafka Server
```bash
vi /etc/systemd/system/zookeeper.service
```
add the following line to the file
```bash
[Unit]
Requires=network.target remote-fs.target
After=network.target remote-fs.target

[Service]
Type=simple
User=kafka
ExecStart=/home/kafka/kafka/bin/zookeeper-server-start.sh /home/kafka/kafka/config/zookeeper.properties
ExecStop=/home/kafka/kafka/bin/zookeeper-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target
```

```bash
vi /etc/systemd/system/kafka.service
```
add the following line to the file
```bash
[Unit]
Requires=zookeeper.service
After=zookeeper.service

[Service]
Type=simple
User=kafka
ExecStart=/bin/sh -c '/home/kafka/kafka/bin/kafka-server-start.sh /home/kafka/kafka/config/server.properties > /home/kafka/kafka/kafka.log 2>&1'
ExecStop=/home/kafka/kafka/bin/kafka-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target
```
# Reload systemd and start service
```bash
systemctl daemon-reload
systemctl enable --now kafka
```

# Testing the Installation

```bash
~/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic TutorialTopic
echo "Hello, World" | ~/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic TutorialTopic > /dev/null
~/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic TutorialTopic --from-beginning
```

#  Installing KafkaT (Optional)

```bash
yum install ruby ruby-devel make gcc patch
sudo gem install kafkat
vi ~/.kafkatcfg
```
add the following lines to the file

```bsah
{
  "kafka_path": "~/kafka",
  "log_path": "/tmp/kafka-logs",
  "zk_path": "localhost:2181"
}
```
```bash
kafkat partitions
```
