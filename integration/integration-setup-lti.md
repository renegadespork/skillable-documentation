---
title: LTI Setup
---
# Setting up LTI Integration with Skillable
**Learning Tools Interoperability (LTI)** is an API standard created by ISM Global that is used on many popular Learning Management Systems.

## Table of Contents
- [Basics of LTI](#basics-of-lti)
- [How to set up LTI on Popular **Learning Management Systems** (LMSs)](#how-to-set-up-lti-on-your-lms)
    - [Canvas](#canvas)
    - [Moodle](#moodle)
    - [Blackboard](#blackboard)

## Basics of LTI
LTI provides a standardardized framework for linking, authenticating, and scoring 3rd party resources with a compatible LMS. When a student launches a lab via LTI, an exchange of information begins between the LMS and Skillable Connect.

### Launching Labs
While Skillable has powerful built-in API, you can also launch labs using the LTI standard. Here is a high-level view of how the launch process works:
<!--
    Mermaid diagram code (remove backslash from 13th line):
    sequenceDiagram
    box rgb(84, 89, 95) Client-side
    participant User
    end
    box rgb(41, 143, 92) Server-side
    participant LMS
    participant Skillable
    end
    rect rgb(30,90,60)
    User->>LMS: Launch Lab
    end
    LMS->>Skillable: Connection Request
    LMS--\>Skillable: Authentication <br> with JWT Keys
    LMS->>Skillable: Launch Request
    Skillable->>LMS: Send Connection URL
    rect rgb(30,90,60)
    LMS->>User: Redirect browser to <br> Lab Console
    end
-->

![Diagram of LTI Launching Flow](./images/lti-launch-diagram01.svg)

### Deep Linking
LTI 1.3 Includes a suite of tools called [LTI Advantage](https://www.imsglobal.org/lti-advantage-overview). One of these tools is called **Deep Linking**, which allows platforms to request content links directly through LTI.

Rather than manually entering all your lab launch links (which is required with LTI 1.1), Deep Linking can view the whole catalog published to your [**API Consumer**](integration-setup-general.md#what-is-an-api-consumer) and fetch links to the specific labs you want to add.
<!-- 
    Mermaid diagram code (remove backslash from 13th line):
    sequenceDiagram
    box rgb(84, 89, 95) Client-side
    participant User
    end
    box rgb(41, 143, 92) Server-side
    participant LMS
    participant Skillable
    end
    rect rgb(30,90,60)
    User->>LMS: Add new LTI Activity
    end
    LMS->>Skillable: Connection Request
    LMS--\>Skillable: Authentication <br> with JWT Keys
    LMS->>Skillable: Deep Link Request
    Skillable->>LMS: Send Catalog
    rect rgb(30,90,60)
    LMS->>User: Show Catalog
    User->>LMS: Select Labs
    end
    LMS->>Skillable: Request Selected Labs
    Skillable->>LMS: Send Lab Links
    rect rgb(30,90,60)
    LMS->>User: LTI Activity Added
    end
-->
![Diagram of Deep Link Flow](./images/deeplink-diagram01.svg)

> **Note**: Skillable supports both LTI 1.1 and LTI 1.3, however we highly recommend you set up LTI 1.3. We recommend only using LTI 1.1 if your platform requires it, since it lacks many features and will soon be depricated.

## How to Set up LTI on your LMS

The following section will walk through how to set up an LTI 1.3 tool for Skillable on your LMS. 

1. First, you will be given some LTI information from Skillable. You will need this information for LTI setup. The information will look like this:
    | Parameter | Value |
    |---|---|
    |LTI 1.3 Login / Connect Url|https://lod-lti-api.labondemand.com/ltiv13/connect/**\[ID\]**|
    |LTI 1.3 Launch Url|https://lod-lti-api.labondemand.com/ltiv13/launch|
    |LTI 1.3 JWKS Url|https://lod-lti-api.labondemand.com/ltiv13/config/jwks/**\[ID\]**|
    |LTI 1.3 DeepLink Url|https://lod-lti-api.labondemand.com/ltiv13/link|

    > **Important**: The "**\[ID\]**" above will be replaced with the unique ID of your [**API Consumer**](integration-setup-general.md#what-is-an-api-consumer). This will be given to you by Skillable during the setup process.

1. Next, find your LMS below and follow the steps:
    - [Canvas](#canvas)
    - [Moodle](#moodle)
    - [Blackboard](#blackboard)

### Canvas

To set up LTI on Canvas:
 
1. Configure the Tool
    1. Go to the **Admin** Section (sidebar)
    1. Select **Developer Keys**
    1. Select the **+Developer Key** button.
    1. Select **+LTI Key** from the drop-down.
    1. Enter the following information: 

        | Parameter	| Value |
        |---|---|
        |Key Name| skillable-lti-13 |
        |Redirect URIs|https://lod-lti-api.labondemand.com/ltiv13/launch|
        |Title|Skillable LTI 1.3|
        |Description|LTI Integration with Skillable Labs|
        |Target Link URI|https://lod-lti-api.labondemand.com/ltiv13/launch|
        |OpenID Connect Initiation|https://lod-lti-api.labondemand.com/ltiv13/connect/**\[ID\]**|
        |JWK Method|Public JWK URL|
        |Public JWK URL|https://lod-lti-api.labondemand.com/ltiv13/config/jwks/**\[ID\]**|
        > **Important**: The "**\[ID\]**" above will be replaced with the unique ID of your [**API Consumer**](integration-setup-general.md#what-is-an-api-consumer). This will be given to you by Skillable during the setup process.
    
    1. Under **LTI Advantage Services**, enable the following:
        - Can create and view assignment data in the gradebook associated with the tool.
        - Can view submission data for assignments associated with the tool.
        - Can create and update submission results for assignments associated with the tool.
        - Can retrieve user data associated with the context the tool is installed in.
    Can update public jwk for LTI services.
    
    1. Under **Placements**, add the following:
        - Link Selection
        - Assignment Selection
        
    1. Expand the **Link Selection** and **Assignment Selection** dropdown menus. Under **both**, enter the following:
        |Parameter|Value|
        |---|---|
        |Target Link URI|https://lod-lti-api.labondemand.com/ltiv13/link|
        |Select Message Type|LtiDeepLinkingRequest|
    1. Select **Save**.
    1. Back on the **Developer Keys** page, note the **Client ID** (number inder the **Details** column). 
    1. Provide the **Client ID** from the previous step and the **Fully Qualified Domain Name (FQDN)** of your Canvas instance to Skillable to complete setup.
    1. Now that the LTI tool is configured, you can test the functionality by either creating a new **Course**, or adding a new **Assignment** to an existing course.
        1. On the Assignments tab of the Course, select the **+Assignment** button.
        1. Set the the **Submission Type** to **External Tool**. 
        1. Select **Find** and select the tool you just set up.
        1. A popup will appear that lists all the labs published to your API Consumer. During the setup process, Skillable will publish a test Lab Profile to your LMS called **Launch and Scoring Test**. Select the checkbox next to it.
            > This popup uses [Deep Linking](http://www.imsglobal.org/spec/lti-dl/v2p0/) to automatically retrieve labs published to your [**API Consumer**](integration-setup-general.md#what-is-an-api-consumer).
        1. Select the **Add Selected Labs** button.
        1. Check **Load this tool in a New Tab**.
            > This is optional, but recommended. Lab Environments often require a lot of screen space and can be difficult to use in a frame.
        1. Finish configuring the rest to your preferences, then select the **Save and Publish** button at the bottom.
        1. Back on the **Assignments** Tab, select the assignment you just created.
            > **Important**: Ensure you are launching as a Student that is enrolled in the course. If the user does not have the LTI role 'membership#Learner' in the Course context, no grade will be sent back.
        1. You should launch into the lab environment. This environment tells you some information about the lab launch. You can manually set a score, select **Apply Score**, then **Submit**.
        1. The lab will end and the score you entered will be submitted to Canvas.

 Congratulations, you've completed the setup! If you encountered any issues, please check out [Integration Troubleshooting and Support](integration-troublshooting-and-support.md).

[Back to top](#setting-up-lti-integration-with-skillable)

### Moodle
To set up LTI on Moodle:
 
> **Note**: These UI steps assume you have the default Theme installed on Moodle. If you've customized your theme, some elements might be in different places.
1. Go to **Site Administration**.
1. Select the **Plugins** tab.
1. Under **Activity Modules**, select **External Tools** > **Manage Tools**.
1. In the **Add tool** box, select **configure a tool manually**.
    Enter the following information:

    | Parameter | Value |
    |---|---|
    |**Tool Settings**| |
    |Tool name|Skillable LTI 1.3|
    |Tool URL|https://lod-lti-api.labondemand.com|
    |LTI version|LTI 1.3|
    |Public key type|Keyset URL|
    |Public keyset|https://lod-lti-api.labondemand.com/ltiv13/config/jwks/**\[ID\]**|
    |Initiate login URL|https://lod-lti-api.labondemand.com/ltiv13/connect/**\[ID\]**|
    |Redirection URI(s)|https://lod-lti-api.labondemand.com/ltiv13/launch|
    |Tool configuration usage|Show in activity chooser and as a preconfigured tool|
    |Default launch container|New window (recommended)|
    |Supports Deep Linking (Content-Item Message)|TRUE|
    |Content Selection URL|https://lod-lti-api.labondemand.com/ltiv13/link|
    |**Services**| |
    |IMS LTI Assignment and Grade Services|User this service for grade sync and column management (required for grade passback)|
    |IMS LTI Names and Role Provisioning|User this service to retrieve members' information as per privacy settings (required for grade passback)|
    |Tool Settings|Use this service.|
    |**Privacy**| |
    |Share launcher's name with tool|Always (recommended)|
    |Share launcher's email with tool|Always (recommended)|
    |Accept grades from the tool|Always (required for grade passback)|
    |Force SSL|TRUE|
    |**Miscellaneous**| |
    |Default organisation ID|Site hostname|
    |Organisation URL|[Your FQDN]|

1. Once the tool is set up, please provide Skillable with the following information from your tool:
    - Platform ID
    - Client ID
    - Public keyset URL
    - Access token URL
    - Authentication request URL
    > Skillable will finish setting up your [**API Consumer**](integration-setup-general.md#what-is-an-api-consumer) with this information.

1. Now that the LTI tool and [**API Consumer**](integration-setup-general.md#what-is-an-api-consumer) are set up, you can test the functionality by either creating a new **Course**, or adding a new **Activity** to an existing course.
    1. On the Course page, ensure **Edit mode** is enabled.
    1. Create a new topic if needed, and select **+ Add an activity or resource**.
    1. Select the LTI tool you created.
        > Under the default Moodle theme, this will use a puzzle piece icon.
    1. Enter an **Activity name** and select **Select content**.
    1. A popup will appear that lists all the labs published to your API Consumer. During the setup process, Skillable will publish a test Lab Profile to your LMS called **Launch and Scoring Test**. Select the checkbox next to it.
        > This popup uses [Deep Linking](http://www.imsglobal.org/spec/lti-dl/v2p0/) to automatically retrieve labs published to your [**API Consumer**](integration-setup-general.md#what-is-an-api-consumer).
    1. Select the **Add Selected Labs** button.
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
    1. When you are ready to launch the lab, on the Activity page, select **Open in a new window**.
        > **Important**: Ensure you are launching as a Student that is enrolled in the course. If the user does not have the LTI role 'membership#Learner' in the Course context, no grade will be sent back.
    1. You should launch into the lab environment. This environment tells you some information about the lab launch. You can manually set a score, select **Apply Score**, then **Submit**.
    1. The lab will end and the score you entered will be submitted to Moodle.

 Congratulations, you've completed the setup! If you encountered any issues, please check out [Integration Troubleshooting and Support](integration-troublshooting-and-support.md).

[Back to top](#setting-up-lti-integration-with-skillable)

### Blackboard 
1. Request a new Blackboard LTI tool from Skillable.
    > Blackboard requires LTI tools be set up by developers, rather than on the LMS itself. Skillable can set up a new LTI tool for Blackboard upon request.
1. Once Skillable has confirmed the tool is created, sign in to your Blackboard instance and select **System Admin**
1. Under the **Integrations** section, select **LTI Tool Providers**
1. Select **Register LTI Advantage Tool**
1. Enter the **Client ID** recieved from Skillable.
1. Select **Submit**.
1. Review the configuration. Most of the settings will be filled out for you.
1. Ensure the **Tool Status** is **Approved**.
1. Under **Institution Policies**, enter the following:
    | Parameter | Value |
    |---|---|
    | User Fields to send | ☑️ Role in Course <br> ☑️ Name <br> ☑️ Email Address |
    | Allow grade service access | Yes (required for grade passback) |
    | Allow Membership Service Access| Yes (required for grade passback) |
1. Select **Submit**.
1. Hover your cursor over the newly created LTI tool. To the right of the name a dropdown menu icon (🔽) will appear. Select it, and select **Manage Placements**.
1. Select **Synchronize Placements** (*top right above the placement table*).
1. Now that the LTI tool is configured, you can test the functionality by either creating a new **Course**, or adding new **Content** to an existing course.
    1. On the **Content** tab of the Course, select **Build Content**.
    1. In the dropdown menu, select the placement name of the new LTI tool. It should be called **Skillable Deeplinking** or something similar.
    1. A new window will open with the Catalog of labs published to your [**API Consumer**](integration-setup-general.md#what-is-an-api-consumer). During the setup process, Skillable will publish a test Lab Profile to your LMS called **Launch and Scoring Test**. Select the checkbox next to it.
    1. Select the **Add Selected Labs** button.
    1. Close the new window. 
    1. Back on the **Content** tab on your Course, refresh the page. The new lab will show up in the list.
    1. Select the Lab to Launch it. 
        > **Important**: Ensure you are launching as a Student that is enrolled in the course. If the user does not have the LTI role 'membership#Learner' in the Course context, no grade will be sent back. <br>
        You can use the **Enter Student Preview** function on Blackboard to simulate this.
    1. You should launch into the lab environment. This environment tells you some information about the lab launch. You can manually set a score, select **Apply Score**, then **Submit**.
    1. The lab will end and the score you entered will be submitted to Blackboard.

 Congratulations, you've completed the setup! If you encountered any issues, please check out [Integration Troubleshooting and Support](integration-troublshooting-and-support.md).

[Back to top](#setting-up-lti-integration-with-skillable)