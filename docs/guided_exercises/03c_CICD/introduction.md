# {{ product.name }} CI/CD Overview

This lab was forked from the KIE Live session delived by Rafael Soares on YouTube found [here](https://youtu.be/AAq8oUvSOUU). This lab has been updated to more current releases and also includes a provisioning of Nexus3 to deploy a Maven Repository in your environment. You do not need to do this if you have a Maven repository you can deploy your artifacts to. This lab will be using the OpenShift provided Pipelines built upon Tekton. With {{ product.name }}, the projects are based on Maven artifact architectures, which provides an easy to build and deploy mechanism for all of your product needs. These can be done in Tekton as seen here, or with your favorite/enterprise provided CI/CD tools.

The project in this lab is a Spring Boot hosted Decision Service that is utilizing the embedded KIE Server that will have a build and deploy triggered through the changes in your code base that are pushed to the specified build branch. With Decision Service projects, it is highly recommended that on all push events you consider a build of the project. This ensures that all the resources that are required are constantly available, breaking changes are identified immediately and if you want to release immediately, it's very easy to do so.

## Lab Overview

Within this lab you will see how you can use OpenShift Pipelines (a.k.a Tekton) to automate the delivery of decision services implemented with {{ product.short }}. In this lab you will see:

- The automation of repeatable decisions using the [DMN](https://www.drools.org/learn/dmn.html) specification;
- Decision tables implementation using XLS.
- Usage of the rules engine based on KIE Server and running on top of SpringBoot
- CI/CD Pipeline implemented using Tekton
- How to configure webhooks in your pipeline to deploy based on changes on a git repository
- Automated tests for decisions (with Test Scenarios ) that are considered during the pipeline execution
- Deployment with zero downtime with OpenShift rolling deployment strategy

## Pre-requisites

- Java 8

- OpenShift 4.10+

- OpenShift Command Line client (`oc`)

- [VSCode](https://code.visualstudio.com/)

- [VSCode Business Automation Extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-extension-red-hat-business-automation-bundle)

## Installing on OpenShift

1. Fork this repository, in jbossdemocentral.

   ![Fork project](support/docs/images/github-fork-project.png?raw=true "Fork project")
2. Clone your fork to your local machine.

   ~~~ shell
   git clone https://github.com/${yourgithubuser}/business-automation-showcase.git
   cd business-automation-showcase
   ~~~

3. Run the provisioning script (Linux/MacOS):

   ~~~shell
   sh provision.sh
   ~~~

At the end you'll get two URLs: one to a client web application and one to use in the GitHub integration settings. Something like:

~~~console
******************************************************************

Use this URL in your GitHub Webhook configuration for automatic deployment
http://el-ba-cicd-event-listener-rhdm-kieserver-cicd.apps.cluster- (...)

Use this URL to access the front-end application:
http://decision-service-webclient-rhdm-kieserver-cicd.apps.cluster- (...)

******************************************************************
~~~

## Configuring the automatic deployment on GitHub

1. To configure the webhook for automated deployment, open your fork in your GitHub. Next, add a new webhook by opening "**Settings** -> **Webhook** -> **Add webhook** button".

   ![Add GitHub webhook](support/docs/images/github-new-webhook.png?raw=true "Add GitHub webhook")

1. Fill the form with the information below:
   1. **Payload URL**:  provided after the provisioning. You can also get it using the command:

    ~~~shell
     echo "$(oc  get route el-ba-cicd-event-listener --template='http://{{.spec.host}}')"
    ~~~

   1. **Content type**: `application/json`
   1. **Secret**: empty
   1. **Which events would you like to trigger this webhook?**: `Just the push event`.

At this point, you should already have a fully automated integration and deployment lifecycle for the business application. Any changes pushed to your repository will trigger the pipeline in your OpenShift cluster.

### Testing GitHub and Pipeline integration

If you run this test, a new deployment should be triggered. The pipeline will deploy the decision service for the first time.

1. In your terminal, access your project folder.

2. Commit and push. You can use this empty commit sample if you need:

   ~~~shell
   git commit -m "an empty commit to test the pipeline" --allow-empty
   git push origin master
   ~~~

3. In OpenShift, access: "**Pipelines** -> **ba-cicd-pipeline** -> **Pipeline Runs** " and check the progress of your application deployment.
    ![Pipeline progress](support/docs/images/ocp-demo-pipeline-run.png?raw=true "Pipeline progress")

## Using the web application

The web application allows you to interact with the deployed rules and decisions in a specific Decision Server (KieServer or Kogito runtime). To use the deployed web app to interact with the deployed decisions, first you need to set the KIE Server URL in the web app settings.

1. The deployed decision service is now deployed and accessible. Get your deployed KIE Server route. You can use the command:

   `echo "http://"$(oc get route business-application-service-route -n rhdm-kieserver-cicd | awk 'FNR > 1 {print $2}')"/rest/server"`

2. Open your web application. The URL was provided in the installation step. If you lost it, use the command

   `oc get route decision-service-webclient --template='http://{{.spec.host}}' -n rhdm-kieserver-cicd`

3. In the web application, click on the settings icon on the top right corner. In the field `Kie Server Base URL`, insert KIE Server URL.
4. You can use the "Test Connection" button to validate the communication between the two services, then Save.
5. You should be able to test the available decisions and rules.

![Decision Result in Web app](support/docs/images/webapplication-dmn-result.png?raw=true "Decision Result in Web app")

With this, the whole demo is now set up and ready to use.

> NOTE: If you get interested in see how this webapp was developed the src code is available [here](https://github.com/rafaeltuelho/decision-service-webclient)

## Extra information

The provisioning script `provision.sh` will:

- Create a new namespace called rhdm-kieserver-cicd
- Install OpenShift Pipelines
- Create the pipeline resources
- Deploy a front-end application that you can use to interact with the decision service once you deploy it.

At the moment there are 4 projects in this repository:

- [decisions-showcase](decisions-showcase/): Decision use cases using Business Rules (Drools) and Decision Logic (DMN)
- [business-application-service](business-application-service/): Spring Boot runtime based Kie Server exposing the API for Decisions provided with this Showcase demo
- [cicd](cicd/): Tekton Pipeline resources to implement a fully automated CI/CD pipeline for your Business Application Services
- [monitoring](monitoring/): working in progress...

To see a detailed instruction on each service and each deployment processes (with images), check:

- [Provisioning and testing the CI/CD Pipeline](cicd/readme.md)
- [Provisioning and testing the webclient application](https://github.com/rafaeltuelho/decision-service-webclient/blob/main/README.md)

### Interested in Kogito?

Check out the [kogito-quarkus](https://github.com/jbossdemocentral/business-automation-cicd-showcase/tree/kogito-quarkus) branch to see this same demo but using Kogito based Decision Services instead of KieServer.
