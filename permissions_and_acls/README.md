# Permissions and ACLs

This guide highlights some of the permission model features that are provided as part of the Catalyze Mobile Backend as a Service (BaaS). The key features covered are:

- Creating custom class entries and uploading files on behalf of another user within an app
- Granting and revoking permissions to clustom classes and files
- Managing groups and their associated permissions

When going through this guide it is important to save the response output of the various calls made via the API. Keep it handy within a text document to refer back to in the event that you miss a stepi, lose an ID value, etc.

## Complete the Getting Started Guide

This guide assumes that you have already completed the Getting Started guide.

## Create a Custom Class

For many of the examples in this guide we need a custom class within an application to test against.
We will refer to the application as "ExampleApp", the same name used in the [Getting Started guide](gettingStarted.md), and the custom class as "ExampleClass". You are free to use whatever names you like for the purposes of this guide.

### From the API:

**May remove this and have it only done via the dash but its not there today.**
**Would move this /auth/all stuff to an authentication and user management guide**

First, get the **appId** for ExampleApp by clicking on the application within the [Application section](https://dashboard-staging.catalyze.io/applications) of the Dashboard.

Next, get a valid session and transient API key.

    curl -H "X-Api-Key: browser developer.catalyze.io 32a384f5-5d11-4214-812e-b35ced9af4d7" -H "Authorization: Bearer 35206c61-b186-4a5a-9979-bce2e2fcaae8" -H "Content-Type: application/json" https://api.catalyze.io/v2/auth/all -X GET

Find the entry matching the **appID** and grab **session** and **apiKey**. Use them in the call below under <sessionToken> and <apiKey>. Fill in the <appId> value as well.

    curl -H "X-Api-Key: browser dev.portal 0b39e848-f9a2-4cad-a58a-25fe53372b36" -H "Authorization: Bearer 22c1af46-0ff0-4c48-8541-4b44455395a7" -H "Content-Type: application/json" https://api.catalyze.io/v2/classes -X POST -d '{"name":"ACLExample", "schema":{"name":"string", "type":"integer"}}'

### From the Dashboard:

Did not see the custom class creation button.

## Sign in as Alice and Bob

**Note:** For the rest of this guide, only API calls can be made to showcase the features.

This step has you get session tokens within the app for Alice (a supervisor) and Bob (a regular user).

    curl -H "X-Api-Key: browser example.app afa81034-0d7c-4874-924a-e13fee59055c" -H "Content-Type: application/json" https://api.catalyze.io/v2/users -X POST -d '{"username":"bob","password":"bob123", "email":{"primary":"ben+bob@catalyze.io"}, "name":{"firstName":"Bob"}}'

Run the following command to log Bob in:

    curl -H "X-Api-Key: browser example.app afa81034-0d7c-4874-924a-e13fee59055c" -H "Content-Type: application/json" https://api.catalyze.io/v2/auth/signin -X POST -d '{"username":"bob","password":"bob123"}'

Grab the **sessionToken**.

Run the following command to log Alice in:

    curl -H "X-Api-Key: browser example.app afa81034-0d7c-4874-924a-e13fee59055c" -H "Content-Type: application/json" https://api.catalyze.io/v2/auth/signin -X POST -d '{"username":"alice","password":"alice123"}'

Grab the **sessionToken**.

## Create Files for Other Users

### Alice uploads a file for Bob

### Bob uploads a file for Alice

### Alice grants Bob Create on File

## ACLs for Custom Classes and Files

### Granting Create access

### Granting Read access

### Granting Update access

### Granting Delete access

### Revoking ACLs

## Managing Groups

### Create a group

### Add user to group

### Set group permissions

### Removoke group permissions

### Remove user from a groups

### Delete a group


