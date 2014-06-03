 # Permissions and ACLs

This guide highlights some of the permission model features that are provided as part of the Catalyze Mobile Backend as a Service (BaaS). The key features covered are:

- Creating custom class entries and uploading files on behalf of another user within an app
- Granting and revoking permissions to clustom classes and files
- Managing groups and their associated permissions

When going through this guide it is important to save the response output of the various calls made via the API. Keep it handy within a text document to refer back to in the event that you miss a stepi, lose an ID value, etc.

## Complete the Getting Started Guide

This guide assumes that you have already completed the Getting Started guide.



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


