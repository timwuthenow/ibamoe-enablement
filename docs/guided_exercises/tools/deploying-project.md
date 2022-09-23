# Deploying the project in KIE Server

It's time to deploy our business application in KIE Server.

## Deployment

We can deploy the project directly in KIE Server without the need to use Business Central. To do so, we can use the available REST API.

1. Open KIE Server REST API. (i.e. http://localhost:8080/kie-server/docs) 

2. Under “KIE Server and KIE container" category select the following:
   
   * `PUT /server/containers/{containerId} Creates a new KIE container in the KIE Server with a specified KIE container ID`
   
3. Click on "Try it out"

4. Insert your project details. The GAV can be found for example, in your `pom.xml`. See an example:
   
   -  **containerId**: mybusinessapp
   -  **body**: 

      ~~~
       {"container-id" : "mybusinessapp",
           "release-id" : {
               "group-id" : "org.kie.businessapp",
               "artifact-id" : "mybusinessapp",
               "version" : "1.0"
           }
       }
      ~~~
     
5. Click on the blue button "Execute". You should get a 201 result as follows:  

  ![](../images/business_automation/tools/ks-deployment-result.png){:width="600px"}

## Testing the Automated Approval Decision

Now, using the KIE server REST API, we'll consume the decision we've just deployed.

1. Under the section **DMN Models** locate:
   
   * `POST /server/containers/{containerId}/dmn Evaluates decisions for given input`
2. Click on try it out
3. Use the following data:
   * **ContainerID**: mybusinessapp
   * **Body:** 

~~~
  {
   "dmn-context": {
       "request type": "urgent",
       "request price": "250"
   }
  }
~~~

## Extra Lab: Business Central

Finally, you can import this project in Business Central. In order to do so, this needs to be a git-based project and Business Central needs to have access to the git repository where the project is stored. The following steps consider a local environment scenario.

1. Access your application folder in the terminal.
2. Initialize the git repository and do the first commit
   * `git init` 
   * `git add -A`
   * `git commit -m "first commit"`
3. With this you can already import the project in Business Central. Open Business Central and select the **import the project** option.
   * In the pop-up, in the **Repository URL** field, you should insert the git repository. If it is on your local machine you can inform something like: `/$PROJECT_DIR/tooling-labs/mybusinessapp`. Confirm the operation.

4. You should see the project. Select it and click the `Ok` button. 

  ![](../images/business_automation/tools/bc-import.png){:width="600px"}

Feel free to explore the project and validate the test scenario and deployment through Business Central.

  ![](../images/business_automation/tools/bc-project.png){:width="600px"}
