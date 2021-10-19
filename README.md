
# kogito-bpmn-quickstarts
## Creating a kogito BPMN project from archetype

    mvn io.quarkus:quarkus-maven-plugin:create \
        -DprojectGroupId=org.jaime.decide.training -DprojectArtifactId=processes-usertasks-example \
        -DprojectVersion=1.0.0-SNAPSHOT -Dextensions=kogito-quarkus
    
## Runtime tooling for developers (Red Hat WIP) 

    <dependency>
     <groupId>org.kie.kogito</groupId>
     <artifactId>runtime-tools-quarkus-extension</artifactId>
     <version>1.11.0.Final</version>
    </dependency>

See https://blog.kie.org/2021/09/developing-business-processes-more-efficiently-with-runtime-tools-quarkus-extension-part-1.html

# Using Infinispan
**JVM variables**
- `quarkus.infinispan-client.server-list=localhost:11222`
- `quarkus.infinispan-client.auth-username=user`
- `quarkus.infinispan-client.auth-password=pwd1`
- `kogito.persistence.data-index.proto.generation=true`
- `kogito.persistence.data-index.proto.generation=true`
# Using Kafka
See https://docs.jboss.org/kogito/release/latest/html_single/#proc-messaging-enabling_kogito-configuring

**Dependencies**

    <dependency>
      <groupId>io.quarkus</groupId>
      <artifactId>quarkus-smallrye-reactive-messaging-kafka</artifactId>
    </dependency>
    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-cloudevents</artifactId>
    </dependency>

**JVM variables**

- `mp.messaging.incoming.kogito_incoming_stream.connector=smallrye-kafka`
- `mp.messaging.incoming.kogito_incoming_stream.topic=travellers`
- `mp.messaging.incoming.kogito_incoming_stream.value.deserializer=org.apache.kafka.common.serialization.StringDeserializer` 
- `mp.messaging.outgoing.kogito_outgoing_stream.connector=smallrye-kafka`
- `mp.messaging.outgoing.kogito_outgoing_stream.topic=processedtravellers`
- `mp.messaging.outgoing.kogito_outgoing_stream.value.serializer=org.apache.kafka.common.serialization.StringSerializer`

**Examples**
https://github.com/kiegroup/kogito-examples/tree/stable/process-kafka-quickstart-quarkus

# Building a Data Index Service
 https://docs.jboss.org/kogito/release/latest/html_single/#con-data-index-service_kogito-configuring
**Infra dependencies**
- Kafka
- Infinispan
    
# Addons
See https://docs.jboss.org/kogito/release/latest/html_single/#ref-kogito-add-ons_kogito-creating-running
https://docs.jboss.org/kogito/release/latest/html_single/#con-kogito-runtime-events_kogito-configuring
## Messaging Addon
    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-messaging</artifactId>
      <version>1.12</version>
    </dependency>

**Infra dependencies**
 - Kafka
 
**JVM Variables**
 - `kogito.events.processinstances.enabled=true`
 - `kogito.events.usertasks.enabled=true`
 - `kogito.events.variables.enabled=true`

**Custom event emitters** \
`org.kie.kogito.event.EventPublisher` interface

## Jobs addon
See https://docs.jboss.org/kogito/release/latest/html_single/#con-jobs-service_kogito-configuring

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-jobs-management</artifactId>
      <version>1.12</version>
    </dependency>
**Infra dependencies**
- Kafka
- Infinispan

**JVM variables**
 - `kogito.service.url=http://localhost:8080`
 - `kogito.jobs-service.url=http://localhost:8085`
 - `kogito.jobs-service.backoffRetryMillis=1000`
 - `kogito.jobs-service.maxIntervalLimitToRetryMillis=60000`
 - `mp.messaging.outgoing.kogito-job-service-job-status-events.bootstrap.servers=localhost:9092`
 - `mp.messaging.outgoing.kogito-job-service-job-status-events.topic=kogito-jobs-events`
 
## Events addon (default impl)
See https://docs.jboss.org/kogito/release/latest/html_single/#con-kogito-runtime-events_kogito-configuring

    <dependency> 
      <groupId>org.kie.kogito</groupId>  
      <artifactId>kogito-addons-quarkus-events-smallrye</artifactId> 
      <version>1.12</version>
    </dependency>
**JVM variables**

**Custom event listeners** \
https://docs.jboss.org/kogito/release/latest/html_single/#proc-event-listeners-registering_kogito-configuring

## Monitoring addon
See https://docs.jboss.org/kogito/release/latest/html_single/#_metrics_monitoring_in_kogito_services

**For elasticsearch** \
See https://docs.jboss.org/kogito/release/latest/html_single/#proc-elastic-metrics-monitoring_kogito-configuring

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-monitoring-elastic</artifactId>
      <version>1.12</version>  
    </dependency>
**Elasticsearch JVM variables**
- `kogito.addon.monitoring.elastic.host=http://localhost:9200`
- `kogito.addon.monitoring.elastic.index=micrometer-metrics`

**For Prometheus** \
See https://docs.jboss.org/kogito/release/latest/html_single/#proc-prometheus-metrics-monitoring_kogito-configuring

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-monitoring-prometheus</artifactId>
      <version>1.12</version>
    </dependency

## Persistence Addon
See https://docs.jboss.org/kogito/release/latest/html_single/#con-persistence_kogito-configuring

**For filesystem**

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-persistence-filesystem</artifactId>
      <version>1.12</version>
    </dependency>

**For Infinispan** \
See https://docs.jboss.org/kogito/release/latest/html_single/#proc-infinispan-persistence-enabling_kogito-configuring

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-persistence-infinispan</artifactId>
      <version>1.12</version>
    </dependency>

**For jdbc**

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-persistence-jdbc</artifactId>
      <version>1.12</version>
    </dependency>

**For mongoDB** \
See https://docs.jboss.org/kogito/release/latest/html_single/#proc-mongodb-persistence-enabling_kogito-configuring

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-persistence-mongodb</artifactId>
      <version>1.12</version>
    </dependency>

**For PostgreSQL** \
See https://docs.jboss.org/kogito/release/latest/html_single/#proc-postgresql-persistence-enabling_kogito-configuring

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-persistence-postgresql</artifactId>
      <version>1.12</version>
    </dependency>

**For Kafka** \
See https://docs.jboss.org/kogito/release/latest/html_single/#proc-kafka-streams-persistence-enabling_kogito-configuring

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-persistence-kafka</artifactId>
      <version>1.12</version>
    </dependency>

## Process Management Console addon
See https://docs.jboss.org/kogito/release/latest/html_single/#con-bpmn-process-management-addon_kogito-configuring

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-process-management</artifactId>
    </dependency>

## Process SVG addon
See https://docs.jboss.org/kogito/release/latest/html_single/#con-bpmn-process-svg-addon_kogito-developing-process-services

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-process-svg</artifactId>
    </dependency>
## Task management addon
See https://docs.jboss.org/kogito/release/latest/html_single/#con-task-console_kogito-developing-process-services

    <dependency>
      <groupId>org.kie.kogito</groupId>
      <artifactId>kogito-addons-quarkus-task-management</artifactId>
      <version>1.12</version>
    </dependency>




