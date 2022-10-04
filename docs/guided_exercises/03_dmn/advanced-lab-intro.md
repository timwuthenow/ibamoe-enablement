# Call Centre - Intro and Use Case

This is an advanced Decision Model & Notation lab that introduces DMN Decision Services, Relations, nested boxed expressions, etc. It also explores a number of different FEEL constructs and expressions like, for example, `list contains`.

## Goals

- Implement a DMN model using the Red Hat DM/PAM DMN editor
- Deploy the existing DMN project to Decision Server

## Problem Statement

In this lab we will create a decision that determines if a call-centre can take an incoming call. Whether a call will be accepted by a certain office depends on:

- The office accepts the call.

- There are employees currently available at the office.

Whether the office can accepts a call depends on: *whether the phone number has been banned.* the purpose of the phone call ("help" or "objection").

## Create a Decision Project

To define and deploy a DMN decision model, we first need to create a new project in which we can store the model. To create a new project:

1. Navigate to [Business Central](https://localhost:8080/business-central)

1. Login to the platform with the provided username and password.

1. Click on **Design** to navigate to the Design perspective.

    ![BC Splash Screen](../images/business_automation/dmn/business-central-design.png){:width="600px"}

1. In the Design perspective, create a new project. If your space is empty, this can be done by clicking on the blue **Add Project** button in the center of the page. If you already have projects in your space, you can click on the blue **Add Project** icon at the top right of the page.

1. Give the project the name `call-centre-decisions`, and the description "Call Centre Decisions".

    ![Create Call Centre Decisions](../images/business_automation/dmn/add-project-call-centre-decisions.png){:width="600px"}

1. With the project created, we can now create our DMN model. Click on the blue **Add Asset** button.

1. In the **Add Asset** page, select **Decision** in the dropdown filter selector.

    ![Add new assets decision](../images/business_automation/dmn/new-asset-decisions-filter.png){:width="600px"}

1. Click on the **DMN** tile to create a new DMN model. Give it the name `call-centre`. This will create the asset and open the DMN editor.

![Create DMN called call-centre](../images/business_automation/dmn/add-dmn-call-centre.png){:width="600px"}

## Next Steps

You can do this lab in 2 ways:

1. If you already have (some) DMN knowledge, we would like to challenge you to build the solution by yourself. After you’ve built solution, you can verify your answer by going to the next module in which we will explain the solution and will deploy it onto the runtime.

1. Follow this step-by-step guide which will guide you through the implementation.
