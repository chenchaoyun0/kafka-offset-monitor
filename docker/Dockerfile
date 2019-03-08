FROM java:8

ENV TZ Asia/Shanghai

VOLUME /opt/project/db
VOLUME /tmp

WORKDIR /opt/project


ADD kafka-offset-monitor-assembly-0.4.7.jar /opt/project


EXPOSE 8086

CMD \
echo "================kafka 地址为：$KAFKA_BROKERS" && \
echo "================zookeeper 地址为：$ZK_HOSTS" && \
echo "================刷新间隔 为：$REFRESH_SECENDS" && \
echo "================保留天数 为：$RETAIN_DAYS" && \
echo "================正在启动 kafka-offset-monitor..." && \
java -Xms800m -Xmx4096m -Xmn1024m -XX:+UseConcMarkSweepGC -XX:+UseCMSCompactAtFullCollection \
     -XX:CMSInitiatingOccupancyFraction=70 -XX:+CMSParallelRemarkEnabled -XX:+CMSClassUnloadingEnabled \
     -XX:SurvivorRatio=8 -XX:+DisableExplicitGC -Xss1024K -XX:PermSize=256m -XX:MaxPermSize=1024m \
     -cp kafka-offset-monitor-assembly-0.4.7.jar \
     com.quantifind.kafka.offsetapp.OffsetGetterWeb \
     --offsetStorage kafka \
     --kafkaBrokers $KAFKA_BROKERS \
     --kafkaSecurityProtocol PLAINTEXT \
     --zk $ZK_HOSTS \
     --port 8086 \
     --refresh $REFRESH_SECENDS.seconds \
     --retain $RETAIN_DAYS.days \
     --dbName ./db/offsetapp_kafka