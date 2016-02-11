# Kafka and Zookeeper

FROM java:openjdk-7-jre

ENV DEBIAN_FRONTEND noninteractive
ENV SCALA_VERSION 2.8.0
ENV KAFKA_VERSION 0.7.2
ENV KAFKA_HOME /src/kafka-0.7.2-incubating-src
ENV KAFKA_072 http://archive.apache.org/dist/kafka/old_releases/kafka-0.7.2-incubating/kafka-0.7.2-incubating-src.tgz

# Install Kafka, Zookeeper and other needed things
RUN apt-get update && \
    apt-get install -y zookeeper wget supervisor dnsutils openjdk-7-jdk && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean && \
    wget -q http://archive.apache.org/dist/kafka/old_releases/kafka-0.7.2-incubating/kafka-0.7.2-incubating-src.tgz -O /tmp/kafka-0.7.2-incubating-src.tgz && \
    mkdir /src && \
    tar xfz /tmp/kafka-0.7.2-incubating-src.tgz -C /src && \
    cd /src/kafka-0.7.2-incubating-src && \
    ./sbt update && \
    ./sbt package && \
    rm /tmp/kafka-0.7.2-incubating-src.tgz

ADD scripts/start-kafka.sh /usr/bin/start-kafka.sh

RUN chmod +x /usr/bin/start-kafka.sh

# Supervisor config
ADD supervisor/kafka.conf supervisor/zookeeper.conf /etc/supervisor/conf.d/

# 2181 is zookeeper, 9092 is kafka
EXPOSE 2181 9092

CMD ["supervisord", "-n"]
