# Permissions and ACLs

This guide highlights some of the permission model features that are provided as part of the Catalyze Mobile Backend as a Service (BaaS). The key features covered are:

- Creating custom class entries and uploading files on behalf of another user within an app
- Granting and revoking permissions to clustom classes and files
- Managing groups and their associated permissions

When going through this guide it is important to save the response output of the various calls made via the API. Keep it handy within a text document to refer back to in the event that you miss a step, lose an ID value, etc.

## Complete the Getting Started Guide

This guide assumes that you have already completed the Getting Started guide.

## Sign in as Alice and Bob

**Note:** For this section of the guide, only API calls can be made to showcase the features.

This step has you get session tokens within the app for Alice (a supervisor) and Bob (a regular user). The commands below assume that you did not change the usernames or passwords for Alice and Bob provided in the Getting Started guide. Change the values accordingly if you adjusted these values previously.

Run the following command to log Bob in:

    curl -H "X-Api-Key: <apiKey>" -H "Content-Type: application/json" https://api.catalyze.io/v2/auth/signin -X POST -d '{"username":"bob","password":"bob123"}'

Record the value of Bob's **sessionToken** for future use.

Run the following command to log Alice in:

    curl -H "X-Api-Key: <apiKey>" -H "Content-Type: application/json" https://api.catalyze.io/v2/auth/signin -X POST -d '{"username":"alice","password":"alice123"}'

Record the value of Alice's **sessionToken** for future use.

