# Create a Custom Class Entry for Another User

This section is very similar in purpose to the previous section, where a user was able to upload a file on behalf of another user. In that section, Alice, a doctor, shared image data with her patient, Bob, a regular user of the application. This section covers the same use case but here custom classes are used to exchange data instead of files.

Values needed to run this example:

| Value | Description |
| -- | -- |
| *apiKey* | The API key for ExampleApp |
| *aliceSessionToken* | a valid session token for Alice obtained from logging in using *apiKey* |
| *bobSessionToken* | a valid session token for Bob obtained from logging in using *apiKey* |
| *bobUsersId* | Bob's *usersId*, obtained when signing Bob in to ExampleApp |

## Alice Creates an Entry for Bob

Run the command below to have Alice create an entry in ExampleClass for Bob.

    curl -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <aliceSessionToken>" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/classes/ExampleClass/entry/<bobUsersId> -d '{"content":{"name":"FromAlice","type":123}}

Example response:

    {"content":{"name":"FromAlice","type":123},"parentId":"e56d261f-8206-4921-8026-95cf55667b30","authorId":"0ebfeab0-b0f7-41a1-9f7e-bac7390eb76c","phi":false,"createdAt":1401793517168,"updatedAt":1401793517168,"entryId":"pWFz8uBYXILw3lWOK4OiisNo6M"}

Record the value of **entryId** to be used in the next section.

## Bob Retrieves the Entry

Once Alice's has created the entry, Bob can manipulate it just like any entry he created, as he is the **owner**. Alice also retains control over the entry as she is the **author**.

Bob will see the new entry when he queries the custom class:

    curl -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <bobSessionToken>" -H "Content-Type: application/json" -X GET 'https://api.catalyze.io/v2/classes/ExampleClass/query?field=name&searchBy=FromAlice'

Bob can fetch the new entry given its **entryId**:

    curl -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <bobSessionToken>" -H "Content-Type: application/json" -X GET 'https://api.catalyze.io/v2/classes/ExampleClass/entry/<entryId>'

Bob can delete the entry given its **entryId**:

    curl -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <bobSessionToken>" -H "Content-Type: application/json" -X DELETE 'https://api.catalyze.io/v2/classes/ExampleClass/entry/<entryId>'

## Bob Creates an Entry for Alice

Trying to have Bob create a custom class entry for Alice will fail as Bob, a regular user, does not have the correct permissions to create entries on Alice's behalf. As a supervisor, Alice has complete control over the application's data, whereas Bob can only view and modify data that he has created or that has been shared with him by another user with sufficient permissions.

For Bob to be able to create data for Alice, we will need to elevate Bob's permissions. See section 7.3 of this guide form information on how to work with Access Control Lists (ACLs).
