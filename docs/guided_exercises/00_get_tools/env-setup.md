# Environment Setup

This section will cover the deployment of some of the tools locally that you will use in these exercises.
**{{ product.name }}**

In order to follow these guided labs, you should have {{ product.short }} 8.0+ in your local environment. {{ product.short }} is flexible in how you design. The developer runtime tools are predominantly focused in VSCode (download [here](https://code.visualstudio.com)) with Maven builds targeted at a more cloud native strategy. There are also runtime options available through the utilization of the KIE Server (Knowledge is Everything) to execute. This enablement session will go through both, but the evolution of the jBPM/Drools projects are definitely more towards the Kogito runtimes versus the Java EE-based KIE Server.

If you need to setup {{ product.short }} locally, you can use this repository to help you get up and running quickly: [{{ product.name }} Environment Setup](https://github.com/timwuthenow/ibamoe-setup). This will provide the files as two different forms, a locally built environment that will persist, or the option to have an ephemeral container that will lose the data stored between sessions. Both forms will provide a Business Central and KIE Server environment for use.

## Maven and Java

These labs are going to assume you are already running Maven 3.6.2+ and OpenJDK 11+. To get these added to your environment, you can follow the steps in this section.

### Java

The assumption in these exercises are that you will utilize a supported JDK. These were tested with `openjdk 11.0.11 2021-04-20`, but other ones are supported. You should be using at least Java 11 with {{ product.name }} to ensure your best experience with the tooling.
### Maven

With {{ product.name }}, the components are built around a Maven architecture predominantly. What this ultimately means is your workstation needs to be able to communicate with one to many different Maven Repositories. These labs will use two in particular, the [Red Hat General Availability repository](https://maven.repository.redhat.com/ga/) and [Maven Central](https://repo1.maven.org/maven2/). You could easily replace the two repositories with a local environment one hosting the Maven dependencies as a mirror or based in a disconnected installation, but this is the easiest developer workflow for acquiring new dependencies. The reason we are pointing to the Red Hat Maven repository, at least in the short term, is that the builds for {{ product.name }} are being deployed there as they are the same binaries used within both the IBM and Red Hat products during the transition of Red Hat Process Automation Manager (RHPAM)/Red Hat Decision Manager (RHDM) from Red Hat into IBM Automation under the name of {{ product.name }} ( {{ product.short }}). The settings.xml file included below will use the local Maven repository at your **USER_HOME**/.m2/repository, which when configuring Maven would be the default M2_HOME that's created.

<details><summary>Example settings.xml file</summary><blockquote>

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <localRepository>${user.home}/.m2/repository</localRepository>
  <interactiveMode>true</interactiveMode>
  <usePluginRegistry>false</usePluginRegistry>
  <offline>false</offline>
  <profiles>
    <!-- Profile with online repositories required by IBAMOE -->
    <profile>
      <id>brms-bpms-online-profile</id>
      <repositories>
        <repository>
        <!-- Red Hat Maven Repository-->
          <id>jboss-ga-repository</id>
          <url>https://maven.repository.redhat.com/ga/</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>false</enabled>
          </snapshots>
        </repository>
      </repositories>
      <pluginRepositories>
        <pluginRepository>
          <id>jboss-ga-plugin-repository</id>
          <url>https://maven.repository.redhat.com/ga/</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
        </pluginRepository>
      </pluginRepositories>
    </profile>
    <profile>
      <id>maven-https</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <repositories>
        <repository>
        <!--Maven Central Repository-->
          <id>central</id>
          <url>https://repo1.maven.org/maven2</url>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
        </repository>
      </repositories>
      <pluginRepositories>
        <pluginRepository>
          <id>central</id>
          <url>https://repo1.maven.org/maven2</url>
          <snapshots>
            <enabled>false</enabled>
          </snapshots>
        </pluginRepository>
      </pluginRepositories>
    </profile>
  </profiles>
  <activeProfiles>
    <!-- Activation of the BRMS/BPMS profile -->
    <activeProfile>brms-bpms-online-profile</activeProfile>
    <activeProfile>maven-https</activeProfile>
  </activeProfiles>
</settings>
~~~

</details>

#### Linux/Windows/Mac

Follow the instructions at [the Maven Community](https://maven.apache.org/install.html) to download and update your path variables to incorporate it into your builds.

#### Mac Alternative

The easiest way to acquire Maven is to use **homebrew** and run the command `brew install maven`, which at the time of writing will install Maven 3.8.6.

## Kafka

The Kafka setup requires `docker-compose` to run locally, to get this capability, you can use either Docker or Podman for this capability. To see which is right for you, [read this article](https://www.redhat.com/sysadmin/podman-compose-docker-compose). Coming later in 2022 will be the labs shifted to using an OpenShift deployment of AMQ Streams to minimize the local installations required.

Event-driven processes can react to the events that happens in the ecosystem. Kafka is an open-source even streaming platform, and currently, one of the most popular tools. In this type of architecture, we have the Kafka `topics` used as the communication layer in between the services. Each service can now be considered a `consumer`or a `producer`, in other words, each service can publish or consume events to/from the `topics`.

IBM supports the integration between {{ product.short }} and AMQ Streams (Kafka). To follow the labs, you should have an accessible Kafka server. The KIE Server (process engine) will communicate with the topics that we will create in the Kafka server. 

If you don't have an environment available you can get a Kafka (Strimzi) server quickly running by using Docker. Let's clone the project to the enablement folder.

1. Create a new folder named `enablement` and access it:

    ~~~shell
    mkdir ~/enablement && cd ~/enablement
    ~~~

1. Clone this repository to your local machine.

    ~~~shell
    git clone https://github.com/hguerrero/amq-examples
    ~~~

    The docker-compose file available in this quickstart should bootstrap everything you need to have your Strimzi up and running: Zookeeper, Kafka server v2.5.0, Apicurio Registry and a Kafka Bridge.

1. Access the `amq-examples/strimzi-all-in-one/` folder:

    ~~~shell
    cd amq-examples/strimzi-all-in-one/
    ~~~

1. Start the Kafka environment:

    ~~~shell
    docker-compose up 
    ~~~

Docker will download the images and start the services for you. You now have a Kafka server running on localhost port 9092. 

## Creating the Kafka topics

In the upcoming labs we will need three topics: `incoming-requests`,`requests-approved` and `requests-denied`. Next, you need to create these topics in your Kafka server. If you are using the containerized option mentioned in this lab, you can create the topics using the `kafka-topics.sh` available in the kafka container.  

1. Open a new terminal and let's using the kafka container we have just started. Enter the Kafka folder:

    ~~~shell
    cd ~/enablement/amq-examples/strimzi-all-in-one /
    ~~~
 
1. Create the following three topics:
 
    ~~~shell
    docker-compose exec kafka bin/kafk a-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic incoming-requests
 
    docker-compose exec kafka bin/kafk a-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic requests-approved
 
    docker-compose exec kafka bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic requests-denied
    ~~~

## Next steps

At this point you should have {{ product.short }}, and Kafka on your environment. Before proceeding to the next steps, make sure you have both {{ product.short }} and Kafka server up and running.
