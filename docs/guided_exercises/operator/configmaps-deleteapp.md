# ConfigMaps

The Operator stores its configuration in a number of *ConfigurationMaps*. These ConfigurationMaps can be used to change more advanced configurations that can not be configured in the KieApp YAML. In the case the Operator upgrades the version of your RHPAM environment, the Operator is aware that one of the ConfigMaps has changed and will make a backup of it during the upgrade.

1.  In the OpenShift Console, open **Workloads → Config Maps**. ![](../images/business_automation/operator/operator-lab-config-maps.png){:width="650px"}

2.  Note that the Operator keeps the current ConfigMaps, and the ones of the last 2 versions.

3.  Click on the `kieconfigs-7.10.1` ConfigMap and open the YAML tab.

4.  Explore the configuration options.

5.  Set the `initialDelaySeconds` of the `livenessProbe` of the Business Central console from 180 to 240.

6.  Click the **Save** button to save the configuration.

7.  Go to "Workloads → Deployment Configs", open the `rhpam-trial-rhpamcentr` Deployment Config and open the YAML tab.

8.  Find the LivenessProbe `initialDelaySeconds` configuration and notice that it’s still set to 180.

9.  Delete the DeploymentConfig. This will have the Operator reconciliation recreate the DC.

10. Open the YAML configuation of this recreated DeploymentConfig.

11. Find the LivenessProbe `initialDelaySeconds` configuration and note that this time it has been set to 240, the value set in the ConfigMap.

#  Deleting an application

Apart from provisioning an RHPAM application, the Operator also allows us to easily delete an application.

1.  Navigate to **Operators → Installed Operators → Business Automation → KieApp**.

2.  Click on the kebab icon of the `rhpam-trial` KieApp and click **Delete**. ![](../images/business_automation/operator/operator-lab-rhpam-trial-delete-kie-app.png){:width="650px"}

3.  Navigate back to **Workloads → Deployment Configs** and note that the RHPAM Deployment Configs have been removed. ![](../images/business_automation/operator/operator-lab-rhpam-trial-dc-deleted.png){:width="650px"}

