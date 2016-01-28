Kafka in Docker
===

This repository provides everything you need to run Kafka in Docker.

Why?
---
The main hurdle of running Kafka in Docker is that it depends on Zookeeper.
Compared to other Kafka docker images, this one runs both Zookeeper and Kafka
in the same container. This means:

* No dependency on an external Zookeeper host, or linking to another container
* Zookeeper and Kafka are configured to work together out of the box

Run
---

**Set the hostname of the docker container to the public DNS name, and set `$ADVERTISED_HOST` to the hostname of the container**

Parameters:
* `ADVERTISED_HOST` - external host 
* `ADVERTISED_PORT` - external port
* `ZK_CHROOT` - zookeeper chroot dir
* `LOG_RETENTION_HOURS` - duration to keep a kafka partition log
* `LOG_RETENTION_BYTES` - number of bytes after which partition log gets deleted if exceeding the set amount
* `NUM_PARTITIONS` - default number of partitions per topic
* `KAFKA_LOGS_DIR` - partition logs dir
* `MAX_MESSAGE_SIZE` - set max message size

```bash
docker run -p 2181:2181 -p 9092:9092 --env ADVERTISED_HOST=`docker-machine ip \`docker-machine active\`` --env ADVERTISED_PORT=9092 bemobile/kafka
```

```bash
export KAFKA=`docker-machine ip \`docker-machine active\``:9092
kafka-console-producer.sh --broker-list $KAFKA --topic test
```

```bash
export ZOOKEEPER=`docker-machine ip \`docker-machine active\``:2181
kafka-console-consumer.sh --zookeeper $ZOOKEEPER --topic test
```

In the box
---
* **bemobile/kafka**

  The docker image with both Kafka and Zookeeper. Built from the `kafka`
  directory.


Public Builds
---

https://registry.hub.docker.com/u/spotify/kafka/

Build from Source
---

    docker build -t bemobile/kafka kafka/

Todo
---

* Not particularily optimized for startup time.
* Better docs


Acknowledgements
---

Based on original work by Spotify et al (spotify/kafka)
