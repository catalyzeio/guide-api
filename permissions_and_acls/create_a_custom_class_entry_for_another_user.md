# Create a Custom Class Entry for Another User

This section is very similar in purpose to the previous section, where a user was able to upload a file on behalf of another user. In that section, Alice, a doctor, shared image data with her patient, Bob, a regular user of the application. This section covers the same use case but here custom classes are used to exchange data instead of files.

## Alice Creates an Entry for Bob

Post to Bob:

    curl -H "X-Api-Key: browser example.app afa81034-0d7c-4874-924a-e13fee59055c" -H "Authorization: Bearer 2151db7d-3abd-4d82-aba7-d110bebce362" -H "Content-Type: application/json" -X POST https://api.catalyze.io/v2/classes/ExampleClass/entry/e56d261f-8206-4921-8026-95cf55667b30 -d '{"content":{"name":"FromAlice","type":123}}

## Bob Retrieves the Entry

Queries the Entry by field vaue:

    curl -H "X-Api-Key: browser example.ap34-0d7c-4874-924a-e13fee59055c" -H "Authorization: Bearer bb022d52-f298-4b34-a316-ca22e20f4a4e" -H "Content-Type: application/json" -X GET 'https://api.catalyze.io/v2/classes/ExampleClass/query?field=name&searchBy=FromAlice'

Get the entry by ID:

    curl -H "X-Api-Key: browser example.app afa81034-0d7c-4874-924a-e13fee59055c" -H "Authorization: Bearer bb022d52-f298-4b34-a316-ca22e20f4a4e" -H "Content-Type: application/json" -X GET 'https://api.catalyze.io/v2/classes/ExampleClass/entry/SIVRIvlStritVzlvHpaOKmyfg8R'

Deletes the Entry by ID:

    curl -H "X-Api-Key: browser example.app afa81034-0d7c-4874-924a-e13fee59055c" -H "Authorization: Bearer bb022d52-f298-4b34-a316-ca22e20f4a4e" -H "Content-Type: application/json" -X DELETE 'https://api.catalyze.io/v2/classes/ExampleClass/entry/SIVRIvlStritVzlvHpaOKmyfg8R'

