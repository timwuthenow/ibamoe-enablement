#  Emmitting Events from Business Processes

In order to be able to finish processes based on new events, we will need to set up our environment.

In this setup we will:

- Check the required configuration in the business project
- Add bpmn components to emit messages

##  Emitting events in a business process

1. Now, we need to _End Message Events_  instead of two _End Events_. In Business Central, open the `cc-limit-approval-app` process.

2. Convert the two _End Events_ to _End Message Events_. It should look like this:

  ![](../images/business_automation/pam_kafka/bc-process-definition-step2.png){:width="600px"}

2. Next, configure the Kafka topic name in the message name for both nodes as following:

	  * `Raise Approved` message name: `requests-approved`
	  * `Raise Denied` message name: `requests-denied`

		See below one of the nodes, the _Raise Denied_ node configuration:

	  	![](../images/business_automation/pam_kafka/bc-end-event-config.png){:width="600px"}

3. 	Save the process definition.

## Configuring the business application

1. In business central, navigate to the **Project Settings -> Deployments -> Work Item handlers**: 

 	 ![](../images/business_automation/pam_kafka/bc-project-task-config.png){:width="600px"}

Observe that there is a task configured named `Send Task`. In PAM 7.10 you need this configuration to be able to use any `Message Events` (ending and throwing) that would emit events. 

## Consuming the events from Kafka topic using Kafka Consumer CLI

In order to validate if our process is emitting processes as we expect, we need to listen to the Kafka topics `requests-approved` and `requests-denied` to validate if the messages were emitted correctly.


1. Open a new terminal tab, and navigate to the Kafka project folder. 

	```
	$ cd ~/enablement/amq-examples/strimzi-all-in-one/
	```

2. Start the Kafka command line tool that allows us to consume events that happen in a topic, and therefore, will allow us to know if RHPAM published the events when the process ended. The tool is `kafka-console-consumer.sh`. Let's check if the process emitted events on the topic `requests-approved`.

	```
	$ docker-compose exec kafka bin/kafka-console-consumer.sh --topic requests-approved --from-beginning --bootstrap-server localhost:9092
	```

##  Testing the solution

To test the solution, we will start a new process instance that will start, be automatically approved, and end without any human interaction. A new process instance should get started whenever you publish a new event on the `incoming-requests` topic, and, when there is an automatic approval, the process will end and publish an event to the `requests-approved` topic. Let's see this in action:

1. Like we did on the first lab, let's start a new process instance by publishing a message in the `incoming-requests` topic. If you canceled the execution of the kafka producer, here's how you can start it:

	```
	docker-compose exec kafka bin/kafka-console-producer.sh --topic incoming-requests --bootstrap-server localhost:9092
	```

	You can use the following data in your event: `{"data" : {"customerId": 1, "customerScore": 250, "requestedValue":1500}}`

2. Once you publish the event, a new process should get started.

3. Now check the terminal where you are consuming the messages in the `requests-approved` topic. You should see a new event published by your process. The event will look like this:

	```
	{"specversion":"1.0","time":"2021-04-14T18:04:42.532-0300","id":"25ba2dd0-a8d0-4cfc-9ba4-d2e556ffb4d0","type":"empty","source":"/process/cc-limit-approval-app.cc-limit-raise-approval/5","data":null}
	```

3. Identify the process ID on the event above. In this example, the process instance that emitted this event was process of ID **5**. 

4. Let's check this same process instance in Business Central. In Business Central, open the **Menu -> Manage -> Process Instances**. 

4. On the left column, filter by "Completed" State. You should see as many instances as the number of events you published on Kafka. 

	![](../images/business_automation/pam_kafka/bc-process-instance-list-filtered.png){:width="600px"}

5. Identify your process instance ID. In this example, instance with id **5**. Select the process instance. 

6. Next, select the tab `Diagram`. You should see something like: 

	![](../../images/business_automation/pam_kafka/bc-lab-two-process-instances.png.png){:width="600px"}
