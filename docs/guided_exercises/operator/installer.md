Operator Wizard Installer
==================

The Business Automation Operator contains an *Operator Installer Console*. This console gives you a *wizard* experience to deploy {{ product.name }} environments.

1.  Go the Business Automation Operator and click in **Installer** link. ![](../images/business_automation/operator/operator-lab-installer-console-route.png){:width="650px"} ![](../images/business_automation/operator/operator-lab-installer-console.png){:width="650px"}

2. Login with Openshift. A page will show up asking for authorization. Select all options and click on "Allow selected permissions".

2.  Give the application the name `my-rhpam-prod`.

3.  Select the `rhpam-production` for the Enviroment.

4.  Check the **Enable Upgrades** checkbox.

5.  Scroll down and set the Username and Password to `pamAdmin`:`redhatpam1`.

6.  Click the **Next** button.

7.  Don’t change any values in the **Security** section. Click on **Next**.

8.  Go through the **Components** section of the installer and observe the possible options. Don’t change any values for now.

9.  Keep clicking next until you reach the **Confirmation** screen and click **Deploy**.

10. Go back to the OpenShift Console.

11. Navigate to **Workloads → Deployment Configs** and observe that a new {{ product.name }} production environment has been deployed. Note that this environment has a PostgreSQL database deployed. Also note that both the Business Central and KIE-Server Deployment Configs have their ReplicationController set to 3 pods. ![](../images/business_automation/operator/operator-lab-installer-rhpam-prod-dc.png){:width="650px"}

12. Go back to the **Operators → Installed Operators → Business Automation → KieApp**.

13. Delete the `my-rhpam-prod` we’ve just deployed with the Installer.

We will now deploy a new production environment using the installer, but this time we will configure our KIE-Server in the wizard and set the replications of the KIE-Server to 2 instead of 3.

1.  Go back to the Business Automation Operator, and open the Wizard.

2.  Create a new {{ product.short }} Production Environment. Continue until you reach the **KIE Servers** screen. ![](../images/business_automation/operator/operator-lab-installer-console-new-kieserver.png){:width="650px"}

3.  Click **Add new KIE Server** and use the following configuration for your KIE-Server. ![](../images/business_automation/operator/operator-lab-installer-console-new-kieserver-configuration.png){:width="800px"}

4.  Click through the rest of the screens until you can press the **Deploy** button to deploy the environment.

5.  Navigate to the **Workloads → Deployment Configs** screen to see your {{ product.short }} production environment, including the KIE-Server you configured.

Conclusion
==========

This concludes the lab on the Business Automation Operator. If you have time left, feel free to explore more features of the operator.
