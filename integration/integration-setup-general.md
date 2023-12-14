---
title: Lab API - Basic Setup
---

# Lab API Setup
Whether you are just starting with Lab API or you want to deepen your integration with Skillable Labs, this guide will walk you through setting up and configuring Lab API on your platform.

## Table of Contents
- [What is an API Consumer?](#what-is-an-api-consumer)
- [Basic Options](#basic-options)
- [How to Request API Access](#how-to-request-api-access)
- [Understanding Limits](#understanding-limits)
- [Advanced Options](#advanced-options)

## What is an API Consumer?
An API Consumer is an entity on Skillable Connect that acts as context for Lab API requests. All Lab API requests are tied to an API Consumer. This means that all lab instances, users, classes, assignments, and reporting will be associated with the API Consumer with which they were created.

Lab Developers can view API Consumer(s) on [Skillable Studio](https://labondemand.com/ApiConsumer). The API Consumer details page shows your API configuration, limits, keys, roles, webhooks, published [Lab Series](https://docs.skillable.com/lod/lab-series.md).

Each request must be authorized with an API key. This key is tied to a specific API Consumer. When a request is sent to the Lab API, the API key tells Skillable Studio which API Consumer is making the request and processes it accordingly.

## Basic Options
These are required options for every API implementation. You will need to provide these to Skillable in order to set up Integration.

| Option | Description | Default Value |
|---|---|---|
| Default Organization | All resources created by this API Consumer will be created in this Organization. | Your Production Organization
| Expires | After this date, API requests to this Consumer will return an "*API Access has Expired*" error. | **Production API:** None. <br> (*requires signed contract*)<br><br> **Testing API:** 3 months after creation.
| Configuration | Defines the consumer type. This preset determines your API limits and functionality. <br><br>**Options:** Integration Testing, Per Series Assignment, Per Instance, Concurrent, Exam, Reporting, Custom | The value is chosen by Skillable based on your consumption model. |
| Consuming Platform | Defines the type of platform that will be integrating with this API. This value is used to prioritize support and development for LMS platforms. | "Other LMS"

## How to Request API Access
1. Review the Basic Options above.
2. During the onboarding process, you will be provided with an **Integration Specialist** contact. Submit a request to them with the [Basic Options](#basic-options) and any other unique integration specifications you require.
3. Our integration specialist will go through the integration setup process with you. This may require providing them with additional information depending on your level of integration. 
    - For integrations using our native **Lab API**, you will be provided with an **API Key**. This is required to authenticate Lab API requests. See our [Lab API Documentation](https://connect.skillable.com/lab/overview/) for more information.
    - For integrations using [LTI](https://www.imsglobal.org/activity/learning-tools-interoperability), see our [LTI Setup Guide](integration-setup-lti.md).

## Understanding Limits
Limits are determined by your API Configuration and your license agreement. Increasing limits beyond defaults require contract modification. Contact your Account Executive for more details.

| Limit | Description | Defaults |
|---|---|---|
| Max RAM Usage | Total concurrent RAM for all running Lab Instances launched from this API Consumer. Once this limit is reached, new lab instances cannot be launched. | **Integration Testing:** Unlimited<br> **Per Series Assignment:** 4 TB<br> **Per Instance:** 4 TB<br> **Concurrent:** [*set by contract*]<br> **Exam:** 4 TB<br> **Reporting**: Unlimited |
| Max Active Lab Instances | Total number of concurrent running Lab Instances launched from this API Consumer. Once this limit is reached, new lab instances cannot be launched. | **Integration Testing:** 5<br> **Per Series Assignment:** 1,000<br> **Per Instance:** 1,000<br> **Concurrent:** Unlimited<br> **Exam:** 1,000<br> **Reporting**: Unlimited |
| Default Active Lab Instances Per User | Number of concurrent Lab Instances a user can launch by default. This can be modified per user using the 'maxActiveLabs' parameter in the [Launch](https://connect.skillable.com/lab/operation/Launch/#!in=query&path=maxActiveLabs&t=request) request. | **Integration Testing:** 1<br> **Per Series Assignment:** 1<br> **Per Instance:** 1<br> **Concurrent:** 1<br> **Exam:** 1<br> **Reporting**: 0 |
| Max Active Lab Instances Per User | Maximum value the 'maxActiveLabs' parameter can be set to in the [Launch](https://connect.skillable.com/lab/operation/Launch/#!in=query&path=maxActiveLabs&t=request) request. | **Integration Testing:** 5<br> **Per Series Assignment:** 5<br> **Per Instance:** 5<br> **Concurrent:** 5<br> **Exam:** 5<br> **Reporting**: 0 |
| Default Saved Labs Per User | Number of Lab Instances a user can have saved by default. This can be modified per user using the 'maxSavedLabs' parameter in the [Launch](https://connect.skillable.com/lab/operation/Launch/#!in=query&path=maxSavedLabs&t=request) request. | **Integration Testing:** 1<br> **Per Series Assignment:** 2<br> **Per Instance:** 2<br> **Concurrent:** 0<br> **Exam:** 0<br> **Reporting**: 0 |
| Max Saved Labs Per User | Maximum value the 'maxSavedLabs' parameter can be set to in the [Launch](https://connect.skillable.com/lab/operation/Launch/#!in=query&path=maxSavedLabs&t=request) request. | **Integration Testing:** 5<br> **Per Series Assignment:** 5<br> **Per Instance:** 5<br> **Concurrent:** 0<br> **Exam:** 0<br> **Reporting**: 0 |
| Default Lab Instance Save Days | Default number of days Saved labs will remain before expiring. | **Integration Testing:** 7<br> **Per Series Assignment:** 7<br> **Per Instance:** 7<br> **Concurrent:** 0<br> **Exam:** 0<br> **Reporting**: 0 |
| Max Lab Instance Save Days | Maximum number of days Saved labs will remain before expiring. | **Integration Testing:** 7<br> **Per Series Assignment:** 7<br> **Per Instance:** 7<br> **Concurrent:** 0<br> **Exam:** 0<br> **Reporting**: 0 |
| Maximum Lab Duration | Maximum number of **minutes** any lab launched via this API Consumer will last. This value overrides any setting on the Lab Profile. | **Integration Testing:** 30<br> **Per Series Assignment:** Unlimited<br> **Per Instance:** Unlimited<br> **Concurrent:** Unlimited<br> **Exam:** Unlimited<br> **Reporting**: 0 |
| Minimum Lab Duration for Billing | Lab Launches that remain under this duration in **minutes** will not be billed. | **Integration Testing:** 0<br> **Per Series Assignment:** 0<br> **Per Instance:** 5<br> **Concurrent:** 0<br> **Exam:** 0<br> **Reporting**: 0 |

> **Important**: These values are subject to vary depending on your contract agreement. Any values not specified in contract will revert to these defaults.

## Advanced Options
Didn't see what you need here? Are you looking for more features and more rich integration?

Check out our [Advanced API Setup](./integration-setup-advanced.md).

[Back to top](#lab-api-setup)