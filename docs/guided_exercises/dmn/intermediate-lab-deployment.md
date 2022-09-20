# Vacation Days - Consuming Decisions

The first thing we should do is deploy the project. We'll deploy it in KIE Server using Business Central.

## Deploying the Decision Service

With our decision model completed, we can now package our DMN model in a Deployment Unit (KJAR) and deploy it on the Execution Server. To do this:

1.  In the bread-crumb navigation in the upper-left corner, click on `vacation-days-decisions` to go back to the project’s Library View.

2.  Click on the **Deploy** button in the upper-right corner of the screen. This will package our DMN mode in a Deployment Unit (KJAR) and deploy it onto the Execution Server (KIE-Server).

3.  Go to the **Execution Servers** perspective by clicking on "Menu → Deploy → Execution Servers". You will see the **Deployment Unit** deployed on the Execution Server.

## Testing DMN Solution

In this section, you will test the DMN solution with Execution Server’s Swagger interface and via Java KIE Client API.

## Testing the solution via REST API

In this section, you will test the DMN solution with KIE Server’s Swagger interface.

The Swagger interface provides the description and documentation of the Execution Server’s RESTful API. At the same time, it allows the APIs to be called from the UI. This enables developers and users to quickly test, in this case, a deployed DMN Service.

1.  Navigate to [KIE Server](http://localhost:8080/kie-server/docs)

2.  Locate the **DMN Models** section. The DMN API provides the DMN model as a RESTful resources, which accepts 2 operations:

    1.  `GET`: Retrieves the DMN model.

    2.  `POST`: Evaluates the decisions for a given input.

3.  Expand the `GET` operation by clicking on it.

4.  Click on the **Try it out** button.

5.  Set the **containerId** field to `vacation-days-decisions` and set the **Response content type** to `application/json` and click on **Execute** ![DMN Swagger Get](../images/business_automation/dmn/dmn-swagger-get.png){:width="600px"}

6.  If requested, provide the username and password of your **Business Central** and **KIE-Server** user.

7.  The response will be the model-description of your DMN model.

Next, we will evaluate our model with some input data. We need to provide our model with the **age** of an employee and the number of **years of service**. Let’s try a number of different values to test our deicions.

1.  Expand the `POST` operation and click on the **Try it out** button

2.  Set the **containerId** field to `vacation-days-decisions`. Set the **Parameter content type** and **Response content type** fields to `application/json`.

3. Pass the following request to lookup the number of vacation days for an employee of 16 years old with 1 year of service (note that the namespace of your model is probably different as it is generated. You can lookup the namespace of your model in the response/result of the `GET` operation you executed ealier, which returned the model description). 

   ~~~
        { "dmn-context":{ "Age":16, "Years of Service":1 } }
   ~~~

4. Click on **Execute**. The result value of the `Total Vacation Days` should be 27.

5. Test the service with a number of other values. See the following table for some sample values and expected output.

  <table>
    <colgroup>
      <col style="width: 33%" />
      <col style="width: 33%" />
      <col style="width: 33%" />
    </colgroup>
    <tbody>
      <tr class="odd">
        <td>
          <p>Age</p>
        </td>
        <td>
          <p>Years of Service</p>
        </td>
        <td>
          <p>Total Vacation Days</p>
        </td>
      </tr>
      <tr class="even">
        <td>
          <p>16</p>
        </td>
        <td>
          <p>1</p>
        </td>
        <td>
          <p>27</p>
        </td>
      </tr>
      <tr class="odd">
        <td>
          <p>25</p>
        </td>
        <td>
          <p>5</p>
        </td>
        <td>
          <p>22</p>
        </td>
      </tr>
      <tr class="even">
        <td>
          <p>44</p>
        </td>
        <td>
          <p>20</p>
        </td>
        <td>
          <p>24</p>
        </td>
      </tr>
      <tr class="odd">
        <td>
          <p>44</p>
        </td>
        <td>
          <p>30</p>
        </td>
        <td>
          <p>30</p>
        </td>
      </tr>
      <tr class="even">
        <td>
          <p>50</p>
        </td>
        <td>
          <p>20</p>
        </td>
        <td>
          <p>24</p>
        </td>
      </tr>
      <tr class="odd">
        <td>
          <p>50</p>
        </td>
        <td>
          <p>30</p>
        </td>
        <td>
          <p>30</p>
        </td>
      </tr>
      <tr class="even">
        <td>
          <p>60</p>
        </td>
        <td>
          <p>20</p>
        </td>
        <td>
          <p>30</p>
        </td>
      </tr>
    </tbody>
  </table>

## Using the KIE Java Client

IBM {{ product.dm }} provides a KIE Java Client API that allows the user to interact with the KIE-Server from a Java client using a higher level API. It abstracts the data marshalling and unmarshalling and the creation and execution of the RESTful commands from the developer, allowing him/her to focus on developing business logic.

In this section we will create a simple Java client for our DMN model.

**IMPORTANT:** If your KIE Server is exposed via https you need to configure the ``javax.net.ssl.trustStore and `javax.net.ssl.trustStorePassword` in the Java client code using the Remote Java API. If not, you may get a `rest.NoEndpointFoundException`. For more information check [this solution](https://access.redhat.com/solutions/5424601) Red Hat's knowledge base.

1.  Create a new Maven Java JAR project in your favourite IDE (e.g. IntelliJ, Eclipse, Visual Studio Code).

2. Add the following dependency to your project: 

   ~~~
	   <dependency> 
	     <groupId>org.kie.server</groupId> 
	     <artifactId>kie-server-client</artifactId> 
	     <version>7.48.0.Final-redhat-00006</version> 
	     <scope>compile</scope> 
	   </dependency>
   ~~~

3. Create a Java package in your `src/main/java` folder with the name `org.kie.dmn.lab`.

4. In the package you’ve just created, create a Java class called `Main.java`.

5. Add a `public static void main(String[] args)` method to your main class.

6. Before we implement our method, we first define a number of constants that we will need when implementing our method (note that the values of your constants can be different depending on your environment, model namespace, etc.): 

   ~~~
     private static final String KIE_SERVER_URL = "http://localhost:8080/kie-server/services/rest/server"; 
     private static final String CONTAINER_ID = "vacation-days-decisions"; 
     private static final String USERNAME = "pamAdmin"; 
     private static final String PASSWORD = "redhatpam1!"; 
   ~~~

7. KIE-Server client API classes can mostly be retrieved from the `KieServicesFactory` class. We first need to create a `KieServicesConfiguration` instance that will hold our credentials and defines how we want our client to communicate with the server: 

   ~~~
   KieServicesConfiguration kieServicesConfig = KieServicesFactory.newRestConfiguration(KIE_SERVER_URL, new EnteredCredentialsProvider(USERNAME, PASSWORD)); 
   ~~~

8. Next, we create the `KieServicesClient`: 

   ~~~
   KieServicesClient kieServicesClient = KieServicesFactory.newKieServicesClient(kieServicesConfig); 
   ~~~

9. From this client we retrieve our DMNServicesClient: 

   ~~~
   DMNServicesClient dmnServicesClient = kieServicesClient.getServicesClient(DMNServicesClient.class); 
   ~~~

10. To pass the input values to our model to the Execution Server, we need to create a `DMNContext`: 

    ~~~
    DMNContext dmnContext = dmnServicesClient.newContext();
    dmnContext.set("Age", 16); dmnContext.set("Years of Service", 1);
    ~~~

11. We now have defined all the required instances needed to send a DMN evaluation request to the server: 

    ~~~
    ServiceResponse<DMNResult> dmnResultResponse = dmnServicesClient.evaluateAll(CONTAINER_ID, dmnContext);
    ~~~

12. Finally we can retrieve the DMN evaluation result and print it in the console:

    ~~~
    DMNDecisionResult decisionResult = dmnResultResponse.getResult().getDecisionResultByName("Total Vacation Days"); System.out.println("Total vacation days: " + decisionResult.getResult()); 
    ~~~

13. Compile your project and run it. Observe the output in the console, which should say: **Total vacation days: 27**


The complete project can be found here: <https://github.com/kmacedovarela/dmn-workshop-labs/tree/master/vacation-days-dmn-lab-client>
