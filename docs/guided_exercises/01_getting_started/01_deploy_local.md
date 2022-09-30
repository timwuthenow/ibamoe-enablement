# Deploying the Project locally as a Quarkus runtime

Now that we have seen a little of the DMN Decision, let's use the combined power of Quarkus ([Learn more here about the Kubernetes-native Java Stack that is the best place for Kogito to run!](https://developers.redhat.com/learn/quarkus)) and Kogito to quickly build and deploy the service using Maven commands and also having the option to deploy it to OpenShift as well.

1. The first thing we're going to do is open a terminal (either in VSCode or whatever method you prefer) and validate that you have Maven and Java installed.

    ~~~bash
    java -version
    ~~~

    *Expected output should be similar to*

    ~~~console
    openjdk version "11.0.11" 2021-04-20
    OpenJDK Runtime Environment AdoptOpenJDK-11.0.11+9 (build 11.0.11+9)
    OpenJDK 64-Bit Server VM AdoptOpenJDK-11.0.11+9 (build 11.0.11+9, mixed mode)
    ~~~

    ~~~bash
    mvn -version
    ~~~

    *Expected output should be similar to:*

    ~~~console
    Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
    Java version: 18.0.2, vendor: Homebrew, runtime: /opt/homebrew/Cellar/openjdk/18.0.2/libexec/openjdk.jdk/Contents/Home
    Maven home: /opt/homebrew/Cellar/maven/3.8.6/libexec
    Default locale: en_US, platform encoding: UTF-8
    OS name: "mac os x", version: "12.6", arch: "aarch64", family: "mac"
    ~~~

2. Now that our environment is has the appropriate resources, let's build and run the service locally. Quarkus does this very simply with the command `mvn quarkus:dev` which will operate the container in development mode. By default it will run on 2 ports when running this command, `8080` and `5005`. These ports can be maintained in the `application.properties` file in `src/main/resources/` or you can modify the `mvn quarkus:dev` command to load those properties from the command line. If you want to do the command line, you can do `mvn quarkus:dev -Dquarkus.http.port=8085 -Ddebug=7007` which would setup your application to run off of port `8085` and be able to attach a remote debugger at `7007`. To change the default host in application.properties add the following line.

    ~~~java
    quarkus.http.port=8085
    ~~~

3. After the project's application.properties has been updated, let's start the Quarkus service by running the following command. This command will run the remote debugger (which we are not using in these labs) on port 6006 so that if you already have another application running on 5005 (the default port) it won't give an error.

    ~~~shell
    mvn quarkus:dev -Ddebug=6006
    ~~~~

4. When the service is ready, it will produce a log similar to the below:

    ~~~log
    $ quick-kogito % mvn quarkus:dev -Ddebug=6006
    [INFO] Scanning for projects...
    [INFO] 
    [INFO] --------------------< com.ibm.sample:quick-kogito >---------------------
    [INFO] Building quick-kogito 1.0.0-SNAPSHOT
    [INFO] --------------------------------[ jar ]---------------------------------
    ...
    2022-09-29 20:48:34,404 INFO  [org.kie.kog.cod.dec.DecisionValidation] (build-23) Initializing DMN DT Validator...
    2022-09-29 20:48:34,405 INFO  [org.kie.kog.cod.dec.DecisionValidation] (build-23) DMN DT Validator initialized.
    2022-09-29 20:48:34,405 INFO  [org.kie.kog.cod.dec.DecisionValidation] (build-23) Analysing decision tables in DMN Model 'pricing' ...
    2022-09-29 20:48:34,410 INFO  [org.kie.kog.cod.dec.DecisionValidation] (build-23)  analysis for decision table 'Base price':
    2022-09-29 20:48:34,412 INFO  [org.kie.kog.cod.dec.DecisionValidation] (build-23)   Decision Table Analysis of table 'Base price' finished with no messages to be reported.
    2022-09-29 20:48:34,494 INFO  [org.kie.kog.qua.com.dep.KogitoAssetsProcessor] (build-38) reflectiveEfestoGeneratedClassBuildItem org.kie.kogito.quarkus.common.deployment.KogitoGeneratedSourcesBuildItem@415815e1
    __  ____  __  _____   ___  __ ____  ______ 
     --/ __ \/ / / / _ | / _ \/ //_/ / / / __/ 
     -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \   
    --\___\_\____/_/ |_/_/|_/_/|_|\____/___/   
    2022-09-29 20:48:35,728 INFO  quick-kogito 1.0.0-SNAPSHOT on JVM started in 2.432s. 
    2022-09-29 20:48:35,728 INFO  Listening on: http://localhost:8085
    2022-09-29 20:48:35,728 INFO  Profile dev activated. Live Coding activated.
    2022-09-29 20:48:35,728 INFO  Installed features: [cdi, kogito-decisions, kogito-predictions, kogito-processes, kogito-rules, resteasy-reactive, resteasy-reactive-jackson, smallrye-context-propagation, smallrye-openapi, swagger-ui, vertx]
    ~~~
    