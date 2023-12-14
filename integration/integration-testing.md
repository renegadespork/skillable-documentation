---
title: Testing and Validation
---

# Testing and Validating Your Integration
It is vital to test and validate each facet of integration before labs are deployed into production. 

Choose your integration method below:
- [Lab API](#lab-api)
- [LTI](#lti)

## Lab API

### Test Launch

#### Using Skillable Connect Tools
This guide will show you how to launch directly from this documentation using the "**Try it**" feature.

Before attempting to launch a lab, you will need the following:
- An **API Key**. This can be obtained from Skillable during the [Integration Setup](integration-setup-general.md) process.
- A lab **Published** to your [API Consumer](./integration-setup-general.md#what-is-an-api-consumer). A lab is published if:
    - \[Studio\] The Lab Profile is marked as **Complete**.
    - \[Studio\] The Lab Series containing the Lab Profile is published to your API Consumer.
    > **Note**: In most cases, a test lab will be automatically published to your API Consumer during the setup process with an **Integration Specialist**.

Once you have confirmed the lab is ready to launch:
1. Go to the [Launch](https://connect.skillable.com/lab/operation/Launch/) request details page.
1. Near the top right corner of the page, select the **Try it** button.
1. Enter your API key under **Security > api_key**.
1. Select **Parameters** and enter the following required information:
    - **labid** - This must match the ID of a lab published to your API Consumer
    - **userid** - This can be any unique ID used to identify the user in your external system.
    - **firstname** - This can be any string.
    - **lastname** - This can be any string.
1. Enter any optional parameters you desire.
1. Select **Send**.
    > **Note**: The full request query is shown next to the **Send** button.
1. If the request went through, you will see a **Status: 200 OK** response. 
1. Expand the **Responses > 200 OK Response** section at the bottom of the page for an explanation of each response parameter.

#### Using Your Own Platform

Labs can be launched by any platform that supports REST API. If you haven't configured integration in your platform / application, all these tests can be run using a test tool like **Postman**, or even terminal tools like PowerShell or cURL. 

Before attempting to launch a lab, confirm **all** of the following:
- Your [API Consumer](./integration-setup-general.md#what-is-an-api-consumer) has been configured by Skillable (see [Integration Setup](integration-setup-general.md)).
- Integration is configured on your platform (or API testing tool).
- The lab is **Published** to your API Consumer. A lab is published if:
    - \[Studio\] The Lab Profile is marked as **Complete**.
    - \[Studio\] The Lab Series containing the Lab Profile is published to your API Consumer.
    > **Note**: In most cases, a test lab will be automatically published to your API Consumer during the setup process with an **Integration Specialist**.    

Once you have confirmed the lab is ready to launch:
1. Send a [Launch](https://connect.skillable.com/lab/operation/Launch/) request with the following parameters: 
    - **labid** - This must match the ID of a lab published to your API Consumer
    - **userid** - This can be any unique ID used to identify the user in your external system.
    - **firstname** - This can be any string.
    - **lastname** - This can be any string.
    - Any optional parameters you desire. The [Launch](https://connect.skillable.com/lab/operation/Launch/) request details page contains a full list of accepted parameters.
1. You will recieve a json response resembling the following:
    ```
    {
        "Result": 1,
        "Url": "https://labondemand.com/console/setup/1b4909d6-0dbe-43db-9ab9-74ee4f913c4e",
        "LabInstanceId": 3896477,
        "Expires": 1337977153,
        "Status": 1,
        "Error": null
    } 
    ```
1. Expand the **Responses > 200 OK Response** section at the bottom of the page for an explanation of each response parameter.

> If you experienced any issues during this process, see the [Launching Issues (Lab API)](integration-troublshooting-and-support.md#launching-issues-lab-api) section of our [Integration Troubleshooting Documentation](integration-troublshooting-and-support.md).

### Lab API Scoring
Skillable connect can send back both high level and granular scoring information to enable powerful feedback and analytics. 

When a scored lab ends, there are two different ways to retrieve the score over API:

1. Method 1: **Manual** - When a scored lab instance has ended, GET a [Details](https://connect.skillable.com/lab/operation/Details/) request. This will return (along with all the Lab Instance details) the overall score, as well as an array of all activity outcomes (if applicable). 
1. Method 2: **Webhooks** - Rather than retrieving scores with an additional request, you can also configure [webhooks](https://docs.skillable.com/lod/api-consumer.md#webhooks) on the [API Consumer](./integration-setup-general.md#what-is-an-api-consumer) to send the Lab Instance details (or a custom response) automatically when the Lab Instance reaches your desired state and triggers the webhook.
    > **Note**: Custom webhooks can be authored to contain any [lab instance variable](https://docs.skillable.com/lod/variables.md).

> If you experienced any issues during this process, see the [Scoring Issues (Lab API)](integration-troublshooting-and-support.md#scoring-issues-lab-api) section of our [Integration Troubleshooting Documentation](integration-troublshooting-and-support.md).

## LTI

### Deep Linking (LTI 1.3 Only)

LTI Advantage includes a feature called [Deep Linking](http://www.imsglobal.org/spec/lti-dl/v2p0/). This allows the LMS to automatically request a catalog of available labs and retrieve direct launch links to the labs you've selected.

> **Important**: Before you begin, ensure LTI 1.3 is configured on your platform (see [LTI Setup](integration-setup-lti.md)).

Select your LMS below: 
- [Canvas](#canvas)
- [Moodle](#moodle)
- [Blackboard](#blackboard)

#### Canvas
1. On the Assignments tab of the Course, select the **+Assignment** button.
1. Set the the **Submission Type** to **External Tool**. 
1. Select **Find** and select the tool you just set up.
1. A popup will appear that lists all the labs published to your [API Consumer](./integration-setup-general.md#what-is-an-api-consumer). During the setup process, Skillable will publish a test Lab Profile to your LMS called **Launch and Scoring Test**. Select the checkbox next to it.
    > This popup uses [Deep Linking](http://www.imsglobal.org/spec/lti-dl/v2p0/) to automatically retrieve labs published to your [**API Consumer**](integration-setup-general.md#what-is-an-api-consumer).
1. Select the **Add Selected Labs** button.
1. Check **Load this tool in a New Tab**.
    > This is optional, but recommended. Lab Environments often require a lot of screen space and can be difficult to use in a frame.
1. Finish configuring the rest to your preferences, then select the **Save and Publish** button at the bottom.

> If you experienced any issues during this process, see the [Deep Linking Issues](integration-troublshooting-and-support.md#deep-linking-issues) section of our [Integration Troubleshooting Documentation](integration-troublshooting-and-support.md).

#### Moodle
1. On the Course page, ensure **Edit mode** is enabled.
1. Create a new topic if needed, and select **+ Add an activity or resource**.
1. Select the LTI tool you created.
    > Under the default Moodle theme, this will use a puzzle piece icon.
1. Enter an **Activity name** and select **Select content**.
1. Expand **Privacy** and ensure the following are checked:
    - Share launcher's name with the tool (*optional, but significantly improves reporting*)
    - Share launcher's email with the tool (*optional, but significantly improves reporting*)
    - Accept grades from the tool (*required for grade passback*)
1. Expand **Grade** and set the following:
    | Parameter | Value |
    |---|---|
    | Type | Point |
    | Maximum Grade | 100 |
    | Grade to pass | 75 |
1. Select **Save and display**.

> If you experienced any issues during this process, see the [Deep Linking Issues](integration-troublshooting-and-support.md#deep-linking-issues) section of our [Integration Troubleshooting Documentation](integration-troublshooting-and-support.md).

#### Blackboard

1. On the **Content** tab of the Course, select **Build Content**.
1. In the dropdown menu, select the placement name of the new LTI tool. It should be called **Skillable Deeplinking** or something similar.
1. A new window will open with the Catalog of labs published to your [**API Consumer**](integration-setup-general.md#what-is-an-api-consumer). During the setup process, Skillable will publish a test Lab Profile to your LMS called **Launch and Scoring Test**. Select the checkbox next to it.
1. Select the **Add Selected Labs** button.
1. Close the new window. 
1. Back on the **Content** tab on your Course, refresh the page. The new lab will show up in the list.

> If you experienced any issues during this process, see the [Deep Linking Issues](integration-troublshooting-and-support.md#deep-linking-issues) section of our [Integration Troubleshooting Documentation](integration-troublshooting-and-support.md).

### Launching
Before attempting to launch a lab, confirm **all** of the following:
- You [API Consumer](./integration-setup-general.md#what-is-an-api-consumer) has been configured by Skillable (see [Integration Setup](integration-setup-general.md)).
- Integration is configured on your platform (see [Integration Setup](integration-setup-general.md)).
- The lab is **Published** to your API Consumer. A lab is published if:
    - \[Studio\] The Lab Profile is marked as **Complete**.
    - \[Studio\] The Lab Series containing the Lab Profile is published to your API Consumer.
    > **Note**: In most cases, a test lab will be automatically published to your API Consumer during the setup process with an **Integration Specialist**.
- (*If using an LMS*) The lab is added to a course (see [LTI Setup](integration-setup-lti.md) for instructions on common LMSs).

Once you have confirmed the lab is ready to launch:
1. Ensure you are logged in as a **Student** on your LMS.
1. Ensure your user account is enrolled in the course where the lab was added.
    > While these steps are not required to launch a lab, Skillable Connect will not return scored unless the user is enrolled as a **Learner** within the context of the course. 
    >
    > This is known as the '**context memberships**' claim in the LTI spec, and it must be equal to '**membership#Learner**'.
1. The lab should appear in your course as one of the following:
    - Activity or Resource (*Moodle*)
    - Assignments (*Canvas*)
    - Content (*Blackboard*)
1. Select the lab and it will launch in a new window (unless you configured the tool to launch embedded).
1. After a short time, the lab console will display "Your lab environment is being built." This means our orchestrator is provisioning all the resources the lab requires to run. Depending on the size of the lab profile, this may take a few miunutes. 
1. Once the lab is done building, you will be connected to the lab.

> If you experienced any issues during this process, see the [Launching Issues (LTI)](integration-troublshooting-and-support.md#launching-issues-lti) section of our [Integration Troubleshooting Documentation](integration-troublshooting-and-support.md).

## LTI Scoring
Skillable connect can send back both high level and granular scoring information to enable powerful feedback and analytics. 

To ensure LTI scoring is working on your platform:
1. Launch a lab that meets **all** of the following requirements:
    - You are logged in as a **Student** on your LMS.
    - Your user account is enrolled in the course where the lab was added.
    - The Lab being launched has at least once scored activity.
        > **Note**: In most cases, a test lab with scored activities will be automatically published to your [API Consumer](./integration-setup-general.md#what-is-an-api-consumer) during the setup process with an **Integration Specialist**.    
1. Once the lab is running, connect to the lab and complete at least one activity.
    > **Note**: You do not need to recieve a passing grade for score pass-back to work. 
1. Select **Submit**.
1. Back on your LMS, ensure a grade/score has been returned.

> If you experienced any issues during this process, see the [Scoring Issues (LTI)](integration-troublshooting-and-support.md#scoring-issues-lti) section of our [Integration Troubleshooting Documentation](integration-troublshooting-and-support.md).

[Back to top](#testing-and-validating-your-integration)