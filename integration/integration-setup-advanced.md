---
title: Lab API - Advanced Setup
---
# Advanced API Setup and Features
This page describes various advanced features and configurations that are not enabled by default, but can be enabled for your organization upon request.

## Table of Contents
- [Multi-Instancing](#multi-instancing)
- [API Keys](#api-keys)
- [Roles](#roles)
- [Misc Features](#miscellaneous-features)

## Multi-Instancing
Skillable can enable users to manage multiple Instances of the same Lab simultaneously.

Once enabled for your Organization, Lab Developers can configure Lab Profiles to allow multiple instances per user. See the **Advanced** section of our [Lab Profile documentation](https://docs.skillable.com/lod/feature-focus/lab-profiles/create.md) for more information.

- **Instance Naming** - Once enabled for your Organization **and** on the Lab Profile users can create custom names for each Lab Instance to help keep track of multiple instances.
> **Note**: To use multi-instancing over API, **Multi-Instance Lab Launch Behavior** must be set to **Generate a deferred launch token** on your [API Consumer](integration-setup-general.md#what-is-an-api-consumer).

## API Keys
- Multiple API Keys - If you require multiple API Keys for security or technical reasons, you can request additional keys from Skillable.
- API Key Expiration - To time limit specific API access, Skillable can set expiration dates on each key. It is recommended to cycle out API keys often. 

## Roles
Custom user roles can specified over API using the 'roleId' parameter of the [Launch](https://connect.skillable.com/lab/operation/Launch/#!in=query&path=roleId&t=request) request. These roles can be given the following permissions:
- **Edit lab profile data** - This is used for Lab development over API. This data can include:
    - Save differencing/delta disks into a new lab profile
    - Save differencing/delta disks in the current lab profile
    - Capture virtualization start states
    - Bypass leaving notes when updating lab environment
- **Edit lab instructions**
- **Edit lab activities**
- **Install virtualization integration services** - Enables Virtual Machines to better integrate with Skillable's platform.
- **Show UI while virtual machines are starting** - Useful for troubleshooting Labs
- **View deployment errors** - Useful for troubleshooting Labs
- **Load developer files into virtual machines**
- **External lab profile assignment creation**
- **Secure network access** - This is used to enable labs to access custom networks in your organization. For more information, see [Lab Network Restrictions](https://docs.skillable.com/lod/lab-networks.md).
- **Edit virtual machine instance RAM/vCPU** - Modifies only the current instance, not the Lab Profile.
- **Map to LTI role** - This is useful for hybrid API/LTI setups. This maps the current Lab API role to a specified [role within the LTI spec](http://www.imsglobal.org/spec/lti-nrps/v2p0#context-membership).

## Miscellaneous  Features
For more rich integration, the following features can be enabled upon request:
- **External User URLs** - You can specify a URL format to allow Skillable Studio to link directly to user accounts on another platform.

    The external user ID will be injected into URL using the replacement value {id}. <br> Ex: https://myexternalsite.com/user/{id}

- **External Class URLs** - You can specify a URL format to allow Skillable Studio to link directly to classes on another platform.

    The external class ID will be injected into URL using the replacement value {id}. <br> Ex: https://myexternalsite.com/class/{id}

- **Lab Instance Sync Notifications** - Allows notifications to be sent to an external platform if recent lab instance data has been significantly altered in a non-standard way (ex. if lab instances are moved or cloned between users), and the external platform should re-synchronize lab instance data that it may have stored locally.
    
    This functionality is uncommon and can usually be satisfied by [Webhooks](https://docs.skillable.com/lod/api-consumer.md#webhooks).
- **User Max Active Labs Behavior** - When a user hits their maximum active lab instances and attempts to launch a new lab, you can choose between the following actions:
    1. Return an error response (default)
    1. Generate a deferred launch token - Skillable Connect creates a deferred launch token and returns a specialized response containing a URL where the user can manage their existing lab instances before continuing to launch a new lab instance.

- **Pendo Integration** - Skillable Studio's Pendo integration can be tied to either **internal** user IDs (Skillable Studio), or **external** user IDs as specified by the 'userid' parameter in the [Launch](https://connect.skillable.com/lab/operation/Launch/#!in=query&path=userid&t=request) request.

- **User Retake Exemptions** - Specified email addresses can be exempted from retake limits set on the [Lab Series](https://docs.skillable.com/lod/lab-series.md). 

[Back to top](#advanced-api-setup-and-features)