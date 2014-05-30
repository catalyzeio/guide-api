# Getting Started Guide

This guide is intended to familiarize you with making API calls against the [Catalyze Mobile backend as a Service](https://catalyze.io/backend-as-a-service/) (BaaS)
and configure an environment that will allow you to quickly follow other Catalyze guides.

Many of the steps in this guide can be completed via the Dashboard in addition to the API. Feel free to follow either approach when setting up the guide environment.

**Guide Requirements**

To get through this and other Catalyze guides you will need the following:

- Developer Account
    * If you haven't already, sign up for a Catalyze [Developer account](https://dashboard.catalyze.io). Make sure to activate your account and successfully log in to the [Dashboard](https://dashboard.catalyze.io) before continuing.
- A terminal that can run **curl** (bundled with OS X, most Linux, Windows via cygwin)

## Log In

To access the API's administrative functions, many of which are mirrored in the Dashboard, a session token is required. If you intend to use the Dashboard for completing this guide you can skip this section.

### From the API:

After verifying that your account is active by successfully logging in to the Dashboard, run the following command to obtain a session token to use on additional calls to the API. You will need to replace the values for **&lt;username&gt;** and **&lt;password&gt;** with

    curl -H "X-Api-Key: browser developer.catalyze.io 32a384f5-5d11-4214-812e-b35ced9af4d7"  https://api.catalyze.io/v2/auth/signin -X POST -d '{"username": "<username>","password": "<password>"}'

Record the **sessionToken** value in the response JSON for use in future calls.

### From the Dashboard:

If you haven't logged in to the [Dashboard](https://dashboard.catalyze.io) yet, go ahead and do it now.

## Create an Organization

Organizations are a collection of applications that are treated as a unit for the purposes of managing administrators and developers. Typically one organization is sufficient for building a small number of applications, but when more and more developers start collaborating on applications it makes sense to create multiple organizations to partition up the applications.

For this step you will be creating an Organization under which to create applications used for testing the API. If you already have created an organization you are free to use it for the rest of this guide. For the purposes of this and future guides we will use the name "ExampleOrg" and suggest you name your organization accordingly.

Note that the user that creates an organization is automatically the administrator of the organization and has complete control over all aspects of the organization as well as the applications within the organization.

### From the API:

Only two fields are required when creating an organization: **name** (the name of the organization) and **description** (a brief description of the organization).

Run the following command to create the organization after replacing the value for **&lt;sessionToken&gt;**:

    curl -H "X-Api-Key: browser developer.catalyze.io 32a384f5-5d11-4214-812e-b35ced9af4d7" -H "Authorization: Bearer <sessionToken>" -H "Content-Type: application/json" https://api.catalyze.io/v2/org -X POST -d '{"name": "ExampleOrg", "description": "An example for testing ACLs."}'

Record the **orgId** value in the JSON response for use in future calls.

### From the Dashboard:

Organizations can be created from the [Organization section](https://dashboard-staging.catalyze.io/account/organization/) of the Dashboard. The **orgId** is shown in the Overview section, as **ID**, when you click on an organization.

## Create an Application

The next step is to create an application within ExampleOrg. We suggest using a new application for working through this and other guides, but you can use an existing one if you like. We will refer to this application as "ExampleApp" for all future guides and references to the application. We suggest you name your application accordingly.

### From the API:

Run the following from the command line, substituting the **&lt;sessionToken&gt;** and **&lt;orgId&gt;** with values from the previous steps:

    curl -H "X-Api-Key: browser developer.catalyze.io 32a384f5-5d11-4214-812e-b35ced9af4d7" -H "Authorization: Bearer <sessionToken>" -H "Content-Type: application/json" https://api.catalyze.io/v2/app -X POST -d '{"orgId":"<orgId>","name": "ExampleApp", "description": "An example for running through Catalyze guides."}'

Record the **appId** value for future calls.

### From the Dashboard:

Go to the [Applications section](https://dashboard-staging.catalyze.io/applications/) of the Dashboard and click **Add new app** from the left navigation bar. Fill in the name ("ExampleApp") and description information. Select ExampleOrg from the list to create the application within that organization.

Once the application is created, note the value of **Application ID** when clicking on the application in the Dashboard. That value will be used as the **appId** in future API calls.

## Create an API Key

All operations against an application require an API key. In most cases only one API key is needed. This section walks through creating an API key.

API keys are inserted into the **X-Api-Key** HTTP header. You can see examples of using the Dashboard's API key in the curl examples above. Yes, the Dashboard is actually an application running on the Catalyze Mobile BaaS!

All API keys have three parts, separated by a space: the **type** (browser, ios, android), **identifier** (a string of your choice) and a **UUID**. All three parts together form a API key and each API key is associated with a single application. API keys can be added and deleted but never updated.

### From the API:

Run the following from the command line, substituting the **&lt;sessionToken&gt;** and **&lt;appId&gt;** with values from the previous steps:

    curl -H "X-Api-Key: browser developer.catalyze.io 32a384f5-5d11-4214-812e-b35ced9af4d7" -H "Authorization: Bearer <sessionToken>" -H "Content-Type: application/json" https://api.catalyze.io/v2/apiKey/<appId> -X POST -d '{"type":"browser","identifier": "example.app"}'

Record the **id** for use in future calls. The full API key is

    browser example.app <id>

where **&lt;id&gt;** is the value of **id** from the response JSON.

### From the Dashboard:

Can't do this on staging yet.

## Create Users

In this section we will create two users within ExampleApp for use in future guides:

- **Alice**: a medical doctor
- **Bob**: Alice's patient

We will be configuring the users such that Alice belongs to the **Supervisor** role, granting her the ability to create, read, update and delete any information within the application. As a regular user, Bob only has access to his data, and data that has been explicitly shared with him.

Managing users and roles to protect Protected Health Information (PHI) is an important aspect of building any Health IT application. We will cover additional cababilities for managing and protecting PHI in future chapters.

### Create Alice

To create the Alice account, fill in the **&lt;emailAddress&gt;** field in the command below and run it. An activation email will be sent to the email address specified. You will not be able to use the account until it has been activated.

    curl -H "X-Api-Key: browser example.app afa81034-0d7c-4874-924a-e13fee59055c" -H "Content-Type: application/json" https://api.catalyze.io/v2/users -X POST -d '{"username":"alice","password":"alice123", "email":{"primary":<emailAddress>}, "name":{"firstName":"Alice"}}'

Record Alice's **usersId** value for future use.

The activation email should appear in the email account provided within a few minutes. Once it arrives, click the activation link in the email and the account will be activated.

### Create Bob

The command to sign up Bob is the same as signing up Alice but you will have to provide a diffrent email than the one used to sign up Alice above.

**Hint:** if you use gmail or Google Apps add a **+** after your regular email handle followed by a unique string (e.g. *you+bob@gmail.com*) to generate a unique address that will get sent to your regular account.

    curl -H "X-Api-Key: browser example.app afa81034-0d7c-4874-924a-e13fee59055c" -H "Content-Type: application/json" https://api.catalyze.io/v2/users -X POST -d '{"username":"bob","password":"bob123", "email":{"primary":"<emailAddress>"}, "name":{"firstName":"Bob"}}'

Record Bob's **usersId** value for future use.

Be sure to activate Bob's account before continuing.

### Sign in as Alice and Bob

Log in as Bob by running the command below.

    curl -H "X-Api-Key: browser example.app afa81034-0d7c-4874-924a-e13fee59055c" -H "Content-Type: application/json" https://api.catalyze.io/v2/auth/signin -X POST -d '{"username":"bob","password":"bob123"}'

Grab the **sessionToken** for future use. Session tokens have a limited lifetime, currently 24 hours, so keep this command handy in case the session times out.

Log in as Alice by running the command below.

    curl -H "X-Api-Key: browser example.app afa81034-0d7c-4874-924a-e13fee59055c" -H "Content-Type: application/json" https://api.catalyze.io/v2/auth/signin -X POST -d '{"username":"alice","password":"alice123"}'

Save the **sessionToken** for later use.

### Make Alice a Supervisor

Update the application with Alice's **usersId** in the supervisors array. This is currently cumbersome via the API. You need to include the application's name, description and current list of users. This information can be retreived by GETing the application. Replace the value for **&lt;appId&gt;** below and run the command.

    curl -H "X-Api-Key: browser developer.catalyze.io 32a384f5-5d11-4214-812e-b35ced9af4d7" -H "Authorization: Bearer 35206c61-b186-4a5a-9979-bce2e2fcaae8" -H "Content-Type: application/json" https://api.catalyze.io/v2/app/<appId> -X GET

The update can then be constructed to add Alice's **usersId** to the **supervisor** array as such:

    curl -H "X-Api-Key:  browser developer.catalyze.io 32a384f5-5d11-4214-812e-b35ced9af4d7" -H "Authorization: Bearer 35206c61-b186-4a5a-9979-bce2e2fcaae8" -H "Content-Type: application/json" https://api.catalyze.io/v2/app/<appId> -X PUT -d '{"name":"ExampleApp","description":"An example for testing ACLs.","permissions":{"supervisor":[<usersId>],"user":[<listOfUsers>]}}'

The result JSON should now show Alice's **usersId** in the **supervisor** array. Making this operation easier is on our roadmap.

## Summary

Created an org, app, api key, custom class and two users. One supervisor.