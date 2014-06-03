# Create a File for Another User

Often in the medical field and in clinical settings it is common that data is being collected and processed *on behalf* of the patient. For example, a radiologist may want to share image data with a patient to show them that they are cancer free. Of course this example involves PHI and Catalyze BaaS makes this scenario very easy and secure.

In the following examples, we will assume that Alice is the medical doctor and is sharing medical data, in the form of files, with Bob, her patient. During the **Getting Started** section we configured ExampleApp to have Alice and Bob as users, with Alice being a Supervisor. If you did not run through the Getting Started steps, please do so before continuing this guide.

Values needed to run this example:

| Value | Description |
| -- | -- |
| *filePath* | The full path of a file to upload. Can be an image, PDF, etc. |
| *apiKey* | The API key for ExampleApp |
| *aliceSessionToken* | a valid session token for Alice obtained from logging in using *apiKey* |
| *bobSessionToken* | a valid session token for Bob obtained from logging in using *apiKey* |
| *aliceUsersId* | Alice's *usersId*, obtained when signing Alice in to ExampleApp |
| *bobUsersId* | Bob's *usersId*, obtained when signing Bob in to ExampleApp |

## Alice Uploads a File for Bob

As a supervisor, Alice has sufficient permission to upload a file and assign its ownership to Bob. The backend storage system will record Alice as the **author** and Bob as the **owner** for any files that Alice uploads to Bob.

Execute this command to upload the file to Bob:

    curl -F "phi=true" -F "file=@<filePath>" -H "X-Api-Key: browser <apiKey>" -H "Authorization: Bearer <aliceSessionToken>" -X POST https://api.catalyze.io/v2/users/<bobUsersId>/files

The call will return with an HTTP 200 along with a JSON body containing **filesId**. Store that value to test that Bob can retrieve the file and then delete it when he is done with it.

Verify that Alice can download the file she just created:

    curl -H "Accept: application/octet-stream" -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <aliceSessionToken>" -X GET https://api.catalyze.io/v2/users/files/<filesId> > img.png

## Bob Receives the File

Once the file is uploaded to Bob, he has complete control over the file as he is the file's **owner**. Likewise, Alice has complete control over the file as she is the **author**. We will now verify that Bob can manipulate the file.

Execute the following command to return Bob's list of files:

    curl -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <bobSessionToken>" -X GET https://api.catalyze.io/v2/users/files

Example result:

    [{"usersid":"e56d261f-8206-4921-8026-95cf55667b30","authorsid":"0ebfeab0-b0f7-41a1-9f7e-bac7390eb76c","filesid":"48e11b50-af79-49df-8744-4fa9717802e9","createdat":"Mon Jun 02 12:10:36 UTC 2014","phi":"true"}]

The **filesId** from the previous section should appear in the list.

Execute the following command to download the file as Bob:

    curl -H "Accept: application/octet-stream" -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <bobSessionToken>" -X GET https://api.catalyze.io/v2/users/files/<filesId> > img.png

Finally, changing the HTTP method to DELETE will allow Bob to delete the file.

    curl -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <bobSessionToken>" -X DELETE https://api.catalyze.io/v2/users/files/<filesId>

Example result:

    {"filesId":"a1925e2a-550c-4241-ba8e-b9b5b9e276b9","createdAt":"Mon Jun 02 12:21:32 UTC 2014","usersId":"e56d261f-8206-4921-8026-95cf55667b30","phi":"true","authorsId":"0ebfeab0-b0f7-41a1-9f7e-bac7390eb76c"}

Note that Alice's is listed as the author of the file.

## Bob Uploads a File for Alice

Trying to have Bob upload a file to Alice will fail as Bob, a regular user, does not have the correct permissions to share a file with Alice. As a supervisor, Alice has complete control over the application's data, whereas Bob can only view and modify data that he has created or that has been shared with him by another user with sufficient permissions.

Here is an example call that will fail with a HTTP 403 and a JSON error message within the result body.

    curl -F "phi=true" -F "file=@<filePath>" -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <bobSessionToken>" -X POST https://api.catalyze.io/v2/users/<aliceUsersId>/files

Error message:

    {"errors":[{"message":"Insufficient privileges to perform the requested action.","code":403}]}

To make the above call work successfully, we will need to elevate Bob's permissions so that he can post files to Alice. See section 7.3 of this guide form information on how to work with Access Control Lists (ACLs).

