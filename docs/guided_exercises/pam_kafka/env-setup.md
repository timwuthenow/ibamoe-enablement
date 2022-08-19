#  Environment Setup

**Red Hat Process Automation Manager**

In order to follow these guided labs, you should have RHPAM 7.9+ in your local environment.

If you need to setup PAM locally, you can use this repository to help you get up and running quickly: [Red Hat PAM Install Demo](https://github.com/jbossdemocentral/rhpam7-install-demo)

**Kafka**

Event-driven processes can react to the events that happens in the ecosystem. Kafka is an open-source even streaming platform, and currently, one of the most popular tools. In this type of architecture, we have the Kafka `topics` used as the communication layer in between the services. Each service can now be considered a `consumer`or a `producer`, in other words, each service can publish or consume events to/from the `topics`.  

Red Hat supports the integration between RHPAM and AMQ Streams (Kafka). To follow the labs, you should have an accessible Kafka server. The KIE Server (process engine) will communicate with the topics that we will create in the Kafka server. 

If you don't have an environment available you can get a Kafka (Strimzi) server quickly running by using Docker. Let's clone the project to the enablement folder. 

1. Create a new folder named `enablement` and access it:

	```
	$ mkdir ~/enablement && cd ~/enablement
	```

1. Clone this repository to your local machine.

	```
	$ git clone https://github.com/hguerrero/amq-examples
	```

	The docker-compose file available in this quickstart should bootstrap everything you need to have your Strimzi up and running: Zookeeper, Kafka server v2.5.0, Apicurio Registry and a Kafka Bridge. 

2. Access the `amq-examples/strimzi-all-in-one/` folder:

	```
	$ cd amq-examples/strimzi-all-in-one/
	```

3. Start the Kafka environment:

	```
	docker-compose up 
	```

Docker will download the images and start the services for you. You now have a Kafka server running on localhost port 9092. 

**Creating the Kafka topics**

In the upcoming labs we will need three topics: `incoming-requests`,`requests-approved` and `requests-denied`. Next, you need to create these topics in your Kafka server. If you are using the containerized option mentioned in this lab, you can create the topics using the `kafka-topics.sh` available in the kafka container. 

1. Open a new terminal and let's using the kafka container we have just started. Enter the Kafka folder:

	```
	$ cd ~/enablement/amq-examples/strimzi-all-in-one/
	```

2. Create the following three topics:

~~~
$ docker-compose exec kafka bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic incoming-requests

$ docker-compose exec kafka bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic requests-approved

$ docker-compose exec kafka bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic requests-denied
~~~

### Next steps

At this point you should have Red Hat PAM, and Kafka on your environment. Before proceeding to the next steps, make sure you have Red Hat PAM and Kafka server up and running.
