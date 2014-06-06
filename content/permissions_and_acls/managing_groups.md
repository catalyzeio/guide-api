# Managing Groups

In this section we will introduce **groups**, group management and granting permissions to groups. Groups are a feature that allows multiple users to be granted uniform access to resources along with the other members of the group. Groups can effectively be used to create new roles beyond the built-in administrator and supervisor roles that the API supports.

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

## Create a Group

Group creation is trivial but requires administrator level credentials. Run the following command to create the ExampleGroup.

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSession>" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/groups -d '{"name":"ExampleGroup"}'

Example result:

    {"name":"ExampleGroup","groupsId":"200902c9-8e84-4863-8f7f-58c5d62b8837"}

Save the value of **groupsId** for future use.

## Add Users to the Group

Either administrator or supervisor permissions are needed to add users to a group. In this example we will have Alice add Bob and herself to ExampleGroup.

Run this command to add Bob:

    curl -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <aliceSessionToken>" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/groups/<groupsId>/users/<bobUsersId>

Run this command to add Alice:

    curl -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <aliceApiKey>" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/groups/<groupsId>/users/<aliceUsersId>

List the group's users to check that Alice and Bob are members of the group:

    curl -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <aliceSessionToken>" -H "Content-Type: application/json" -X GET https://api.catalyze.io/v2/groups/<groupsId>/members

Example result:

    ["0ebfeab0-b0f7-41a1-9f7e-bac7390eb76c","e56d261f-8206-4921-8026-95cf55667b30"]

Alice's **usersId** and Bob's **usersId** should appear in the list returned.

## Granting ExampleGroup Full Permissions on Files

The previous section shows how ACLs work for individual users. A nice feature of the API is that the ACL functionality works exactly the same for users and groups. The following command will give the group full permissions on files. The call is exactly the same as the one we used to give Bob these permissions but here we substitute Bob's **usersId** for the **groupsId* for the group we just created.

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSessionToken>" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/acl/core/files/<groupsId> -d '["create","read","update","delete"]'

Any member of the group now can perform any operation against any other user's files within the application. New members added to the group will receive these permissions as well. Removing a user from the group will revoke their permissions.

Note that Alice is in the group and thus has full permissions on files in two ways: her role (supervisor) and her group membership in ExampleGroup. These are independent permissions, meaning that if we remove Alice from the group she will still retain full control over files from her role as a supervisor.

## Granting ExampleGroup Full Permissions on a Custom Class

The call to grant group permissions to ExampleClass is the same as in the previous section, again swapping Bob's usersId for the groupsId. Again, note that the route changes slightly from the previous call (**/core/files** becomes **/custom/&lt;className&gt;**).

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSessionToken>" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/acl/custom/ExampleClass/<groupsId> -d '["create","read","update","delete"]'

The above call results in Bob having full permissions against the custom class named ExampleClass. Alice effectively has the same permissions as she already had as a supervisor.

## Removoke Group Permissions

Revoking group permissions works in the same manner as revoking user permissions. This call will revoke the group's delete permissions for files:

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSessionToken>" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/acl/core/files/<groupsId> -d '["delete"]'

Here we revoke delete permission but create, read and update are retained.

## Remove a User from a Group

Removing users from groups is simply a matter of changing the method to DELETE in the call used above to add Bob to the group:

    curl -H "X-Api-Key: <apiKey>" -H "Authorization: Bearer <aliceSessionToken>" -H "Content-Type: application/json" -X DELETE https://api.catalyze.io/v2/groups/<groupsId>/users/<bobUsersId>

Bob is no longer a member of the group and will lose all associated permissions, unless he has obtained permissions in some other manner (i.e. through a role, membership in another group or individual user permission grants).

## Delete a Group

Like creating a group, deleting a group requires administrator-level credentials. Run the following command to delete a group:

    curl -H "X-Api-Key: <adminApiKey>" -H "Authorization: Bearer <adminSessionToken>" -H "Content-Type: application/json" -X DELETE https://api.catalyze.io/v2/groups/<groupsId>

After the call, all the permissions inherited by the users of the group will be lost. In our example, Bob will no longer have full file permissions or full custom class permissions. Alice will still maintain these permission as she inherits them from her role as a supervisor.

