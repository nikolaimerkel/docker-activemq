FROM openjdk:8-jre

ENV ACTIVEMQ_VERSION 5.14.3
ENV ACTIVEMQ apache-activemq-$ACTIVEMQ_VERSION
ENV ACTIVEMQ_TCP=61616 ACTIVEMQ_AMQP=5672 ACTIVEMQ_STOMP=61613 ACTIVEMQ_MQTT=1883 ACTIVEMQ_WS=61614 ACTIVEMQ_UI=8161

ENV ACTIVEMQ_HOME /opt/activemq

RUN set -x && \
    curl -s -S https://archive.apache.org/dist/activemq/$ACTIVEMQ_VERSION/$ACTIVEMQ-bin.tar.gz | tar xvz -C /opt && \
    ln -s /opt/$ACTIVEMQ $ACTIVEMQ_HOME && \
    useradd -r -M -d $ACTIVEMQ_HOME activemq && \
    chown -R activemq:activemq /opt/$ACTIVEMQ && \
    chown -h activemq:activemq $ACTIVEMQ_HOME

USER activemq
COPY activemq.xml $ACTIVEMQ_HOME/conf/
COPY assets/activemq-k8s-discovery-1.0.2-jar-with-dependencies.jar $ACTIVEMQ_HOME/lib/
COPY assets/amqp-example-0.1-SNAPSHOT.jar $ACTIVEMQ_HOME/


WORKDIR $ACTIVEMQ_HOME
# EXPOSE $ACTIVEMQ_TCP $ACTIVEMQ_AMQP $ACTIVEMQ_STOMP $ACTIVEMQ_MQTT $ACTIVEMQ_WS $ACTIVEMQ_UI
# TODO: Numeric for OpenShift
EXPOSE 61616 5672 61613 1883 61614 8161

USER root
VOLUME $ACTIVEMQ_HOME/data
CMD ["/bin/sh", "-c", "bin/activemq console"]
