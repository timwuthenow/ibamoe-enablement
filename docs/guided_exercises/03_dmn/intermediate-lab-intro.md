# Vacation Days - Use case and project creation

In this lab you'll try out the combination of DMN decision tables with literal expressions. You will also explore a number of different FEEL constructs and expressions like, for example, ranges. Finally, you'll learn how to use the KIE Java Client to consume decisions.

## Goal

- Implement a DMN model using the Red Hat DM/PAM DMN editor
- Deploy the existing DMN project to Decision Server
- Consume the DMN project using the REST API
- Consume the DMN project using a Java API

## Problem Statement

In this lab we will create a decision that determines the number of vacation days assigned to an employee. The number of vacation days depends on age and years of service.

- Every employee receives at least 22 days.

- Additional days are provided according to the following criteria:

    1. Only employees younger than 18 or at least 60 years, or employees with at least 30 years of service will receive 5 extra days;

    1. Employees with at least 30 years of service and also employees of age 60 or more, receive 3 extra days, on top of possible additional days already given;

    1. If an employee has at least 15 but less than 30 years of service, 2 extra days are given. These 2 days are also provided for employees of age 45 or more. These 2 extra days can not be combined with the 5 extra days.

## Link {{ product.sandbox }} to your GitHub account and create a project in GitHub using features coming soon

If you use the following URL for the [{{ product.sandbox }}](https://kiegroup.github.io/kogito-online-staging/kie-sandbox-accelerators/#/) you can get an early look at the work that's going on to create a new repository around starting a model from the model out. To do this, you need to make sure your connection to GitHub is active with the token and from there you can jump right in!

### Link GitHub to your {{ product.sandbox }}

In this section we will link your GitHub account to the KIE Sandbox so we can easily synchronize changes in DMN with GitHub and our tooling, in this case {{ product.sandbox }}.

1. First click the Gear icon and validate that your GitHub token is in, if not, follow the instructions to connect your public GitHub account to the {{ product.sandbox }}.

    ![Github setup](../99_images/business_automation/dmn/github-token-setup.png)

1. Insert your token, if it is not already there to connect your account. There is a link on the page *Create a new token* that will bring you to the GitHub page to do so.

    ![Token](../99_images/business_automation/dmn/github-token-setup.png)

1. Click *Generate new token* to create a new token that will be used by {{ product.sandbox }}

    ![Token Generate](../99_images/business_automation/dmn/generate-token.png)

1. You can use similar properties to the token created below in the screenshot, but the main 2 to have right now are *repo* and *gist* - the others can be beneficial if you reuse this token for other purposes too, but not required. You can change the date to never expiring or be as short as you want. Once the token is generated though, that is the only time you will see the actual token value.

    - *Name*: Name your token a unique name from any previously created
    - *Expiration*: This can either be a set time period, up to 1 year or never expiring
    - The checkboxes you need are **repo** and **gist** to get the full benefit of {{ product.sandbox }}

    ![Token Properties](../99_images/business_automation/dmn/token-properties.png)

1. Use the copy button that's created with the Token to use in {{ product.sandbox }}.

    ![Copy token](../99_images/business_automation/dmn/token-created.png)

1. Return to {{ product.sandbox }} and insert the Token into the wizard.

    ![Insert token](../99_images/business_automation/dmn/insert-token.png)

1. When your token is pasted, the {{ product.sandbox }} will return a similar screen to below towards your GitHub account.

    ![Successfully linked Git](../99_images/business_automation/dmn/successful-git.png)

---------------

### Create a DMN Model and Create a Project in {{ product.sandbox }}

Now that our account is linked, let's go ahead and create a new DMN model and then later produce a project for it that will reside in our GitHub repositories. {{ product.sandbox }} provides an excellent way to both *push* and *pull* from your GitHub repositories. Right now, the {{ product.sandbox }} can just pull from public repositories. This is something that's being actively developed, so keep checking back into [{{ product.sandbox }}'s Page]({{ sandbox.production }}) for more recently released features frequently!

1. When navigating from {{ product.experimental }}

## Create a Decision Project using Business Central (the Old Way)

To define and deploy a DMN decision model, we first need to create a new project in which we can store the model. To create a new project:

1. Navigate to [Business Central](http://localhost:8080/business-central)

1. Login to the platform with the provided username and password.

1. Click on **Design** to navigate to the Design perspective.

    ![BC Splash Screen](../99_images/business_automation/dmn/business-central-design.png){:width="600px"}
1. In the Design perspective, create a new project. If your space is empty, this can be done by clicking on the blue **Add Project** button in the center of the page. If you already have projects in your space, you can click on the blue **Add Project** icon at the top right of the page.

1. Give the project the name `vacation-days-decisions`, and the description "Vacation Days Decisions".

    ![Create Vacations Days Project](../99_images/business_automation/dmn/add-project-vacation-days-decisions.png){:width="600px"}

1. With the project created, we can now create our DMN model. Click on the blue **Add Asset** button.

1. In the **Add Asset** page, select **Decision** in the dropdown filter selector.

    ![Filter Assets](../99_images/business_automation/dmn/new-asset-decisions-filter.png){:width="600px"}

1. Click on the **DMN** tile to create a new DMN model. Give it the name `vacation-days`. This will create the asset and open the DMN editor.

    ![Create DMN Vacation Days](../99_images/business_automation/dmn/add-dmn-vacation-days.png){:width="600px"}

## Next Steps

You can do this lab in 2 ways:

1. If you already have (some) DMN knowledge, we would like to challenge you to build the solution by yourself. After you’ve built solution, you can verify your answer by going to the next module in which we will explain the solution and will deploy it onto the runtime.

1. Follow the next step with contains a step-by-step guide and will guide you through the implementation.
