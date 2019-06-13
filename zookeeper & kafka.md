+ zookeeper
`sudo nano /etc/systemd/system/zookeeper.service

```
[Unit]
Description=ZooKeeper Service
Documentation=http://zookeeper.apache.org
Requires=network.target
After=network.target

[Service]
Type=forking
User=root
Group=zookeeper
ExecStart=/opt/zookeeper/bin/zkServer.sh start
ExecStop=/opt/zookeeper/bin/zkServer.sh stop
ExecReload=/opt/zookeeper/bin/zkServer.sh restart
WorkingDirectory=/var/lib/zookeeper

[Install]
WantedBy=default.target
```

+ kafka
`sudo nano /etc/systemd/system/kafka.service`
```
[Unit]
Description=Apache Kafka server (broker)
Documentation=http://kafka.apache.org/documentation.html
Requires=network.target remote-fs.target
After=network.target remote-fs.target kafka-zookeeper.service

[Service]
Type=simple
User=root
ExecStart=/opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties
ExecStop=/opt/kafka/bin/kafka-server-stop.sh

[Install]
WantedBy=multi-user.target
```

+ RocksDB
`sudo apt-get install python-virtualenv python-dev librocksdb
