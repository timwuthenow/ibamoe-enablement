#  Red Hat PAM Kafka extension

In order to be able to start processes based on new events, we will need to configure the Red Hat PAM Kafka extension.

The Red Hat PAM Kafka extension allows the KIE Server (process and decision engine) to react to events and publish events to kafka topics.

**INFO:** There are several options in PAM to customize the Kafka address, topic names, etc. In our case, we’re using the default Kafka address, which is, localhost:9092. More customization information can be found in the official Red Hat product documentation: [Configuring a KIE Server to send and receive Kafka messages from the process.](https://access.redhat.com/documentation/en-us/red_hat_process_automation_manager/7.10/html-single/integrating_red_hat_process_automation_manager_with_other_products_and_components/index#kieserver-kafka-proc_integrating-amq-streams)

In this setup steps, we will configure PAM only in the _server level_ - _we are not yet configuring the business project_. We will see how to configure the project as we move forward on the labs.

##  Enabling the Kafka extension

We can configure the engine to support different capabilities. In order to enable processes to be started through eventing, we only need to enable the extension via system property. 

1. With PAM up and running, execute: 

~~~
$JBOSS_HOME/bin/jboss-cli.sh -c
[standalone@localhost:9990 /] /system-property=org.kie.kafka.server.ext.disabled:add(value=false)
[standalone@localhost:9990 /] :shutdown(restart=true)
~~~

The first command will enable the Kafka extension. Next, we're reestarting EAP so that the new configuration is active. You can check EAP logs to confirm it is restarting.

The following output will show up in PAM logs: 

~~~
INFO  [org.kie.server.services.impl.KieServerImpl] (ServerService Thread Pool -- 74) Kafka KIE Server extension has been successfully registered as server extension
~~~

This is the only configuration you will need in a server level to be able to start processes using events.
