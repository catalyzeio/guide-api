# ACLs for Custom Classes and Files

In this section, we will manipulate the access control lists (ACLs) within ExampleApp to allow a regular user, Bob, to perform various operations against other users that would otherwise be impossible with default permissions. In the case of a regular user like Bob, the only data that he can manipulate (create, read, update or delete - or CRUD) by default is data he created originally, or that has been shared with him by another user, as in the previous two sections. This section shows how we can leverage ACLs to provide users with additional permissions within the application.

Values needed to run this example:

| Value | Description |
| -- | -- |
| *adminApiKey* | The transient API key for ExampleApp from calling **/auth/all** (see "Get an Admin-level Session token and API Key" under "Getting Started")|
| *adminSessionToken* | The session token from **/auth/all** |
| *apiKey* | The API key for ExampleApp |
| *aliceSessionToken* | a valid session token for Alice obtained from logging in using *apiKey* |
| *bobSessionToken* | a valid session token for Bob obtained from logging in using *apiKey* |
| *aliceUsersId* | Alice's *usersId*, obtained when signing Alice in to ExampleApp |
| *bobUsersId* | Bob's *usersId*, obtained when signing Bob in to ExampleApp |

Within an application, a user can have create, read, update and delete permissions (CRUD) within files and custom classes.

Here is a summary of the permission types:

| Permission | Description |
| -- | -- |
| *create* | The ability to create data across the application. |
| *read* | The ability to read data across the application. Can list and query data items as well. |
| *update* | The ability to update any data item within the application. |
| *delete* | The ability to delete any data item within the application. |


## Granting Create on Files

In [section 7.1](create_a_file_for_another_user.md), Bob attempted to upload a file to Alice but failed. To make this work, the application administrator can grant Bob create permission on files within the application.

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSessionToken>" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/acl/core/files/<bobUsersId> -d '["create"]'

Bob can now create a file for Alice:

    curl -F "phi=true" -F "file=@/Users/uphoff/Desktop/img.png" -H "X-Api-Key: <apiKy>" -H "Authorization: Bearer <bobSessionToken>" -X POST https://api.catalyze.io/v2/users/<aliceUsersId>/files

The drawback here is that Bob can now create files for **any** user, not just Alice. An upcoming release will allow entity-level permission granting, allowing Alice to grant Bob, and only Bob, the ability to create files against herself.

The key concept here is that ACLs currently function at the **model** level within an application. Currently a model is either a custom class or a file. If a user has been granted a permission on a model it is valid only for that model. We will be extending ACLs to our clinical data models (medications, problems, procedures, etc) in time.

## Granting Full Permissions on Files

If we wanted to give Bob full control over files within the application we could extend his permissions to include read, update and delete permissions.

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSessionToken>" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/acl/core/files/<bobUsersId> -d '["read","update","delete"]'

Bob now can perform any operation against any other user's files within the application.

## Granting Full Permissions on a Custom Class

Granting permission against specific custom classes is also possible. The route is changes slightly (**/core/files** becomes **/custom/&lt;className&gt;**) but otherwise the call is the same:

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSessionToken>" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/acl/custom/ExampleClass/<bobUsersId> -d '["create","read","update","delete"]'

The above call results in Bob having full permissions against the custom class named ExampleClass.

## Revoking ACLs on Files

Revoking ACLs is a matter of simply changing the POST from the above examples into a DELETE. The following command will revoke Bob's create permissions on files.

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSessionToken>" -H "Content-Type: application/json" -X DELETE https://api.catalyze.io/v2/acl/core/files/<bobUsersId> -d '["create"]'

Revoke the remaidner of the permissions with this command:

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSessionToken>" -H "Content-Type: application/json" -X DELETE https://api.catalyze.io/v2/acl/core/files/<bobUsersId> -d '["create","read","update","delete"]'

After running the above commands Bob is back to having default permissions on files. He can only manipulate his own data however any files that he created while he held create permissions will still be accessible to him.

## Revoking ACLs on Custom Classes

Revoking permissions on ExampleClass is almost the same as revoking file permissions, aside from a few changes to the route:

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSessionToken>" -H "Content-Type: application/json" -X DELETE https://api.catalyze.io/v2/acl/custom/ExampleClass/<bobUsersId> -d '["create","read","update","delete"]'
