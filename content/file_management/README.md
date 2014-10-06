# File Management #


##Introduction##

File uploads expose a secure S3-esque bucket structure in a HIPAA compliant fashion. If you require blob or binary storage, this is your best bet. Combined with ACLs, provide access to files only to the users in your application that need it.

**Pre-requisites**

To get through this and other Catalyze guides you will need the following:

- Developer Account
    * If you haven't already, sign up for a Catalyze [Developer account](https://dashboard.catalyze.io). Make sure to activate your account and successfully log in to the [Dashboard](https://dashboard.catalyze.io) before continuing.
    * You'll also want to have gone through [Getting Started](https://docs.catalyze.io/guides/api/latest/getting_started/README.html), at least to the point where  you have an application, api keys and know how to generate a session token with your login credentials. Want to go faster? [Follow the Instructions Here]()(https://docs.catalyze.io/api/latest/#sign-in)
    *


###Load your files###

For our DocClock app, users have the option to upload a picture of their driver's license and insurance card at appointment scheduling to expedite the registration process from their browser. You want to load these files for a specific user that already is already signed in and has a valid session token for your application.

This snippet of Javascript would allow you to write your image to Catalyze's file storage at ```POST /users/files```, where file is an binary object stored in the file element on your web page:

```var request = new XMLHttpRequest();```

```request.open('POST', 'https://api.catalyze.io/v2/users/files');```

```request.setRequestHeader('X-Api-Key', '<YOUR API KEY HERE>');
request.setRequestHeader('Authorization', 'Bearer <YOUR SESSION TOKEN HERE>');
request.setRequestHeader('Accept', 'application/json');```

```request.onreadystatechange = function () {
  if (this.readyState === 4) {
    console.log('Status:', this.status);
    console.log('Headers:', this.getAllResponseHeaders());
    console.log('Body:', this.responseText);
  }
};```

```var form = new FormData();
form.append("file", document.getElementById("file").files[0]);
form.append("phi", false);
request.send(form);```

Once you POST this, you should receive back the file ID like so:

```{
    "filesId": "123abcFileId"
}```

Now, for the user to retrive this file, it's as simple as running a GET query that asks for the file returned to us in ```filesId``` above.

```var request = new XMLHttpRequest();```

```request.open('GET', 'https://api.catalyze.io/v2/users/files/123abcFileId');```

```request.setRequestHeader('X-Api-Key', '<YOUR API KEY HERE>'); request.setRequestHeader('Authorization', 'Bearer <YOUR SESSION TOKEN HERE>'); request.setRequestHeader('Accept', 'application/json');```

```request.onreadystatechange = function () { if (this.readyState === 4) { console.log('Status:', this.status); console.log('Headers:', this.getAllResponseHeaders()); console.log('Body:', this.responseText); } };```

```request.send();````

###ACLs and User Provisioning###

Now that we have our file loaded in the system, we want to let one of our DocBook registration staff retrieve the Driver's License photo and perform some pre-authorization before their visit. We want to give the registration staff some access for files, but only for specific files in their queue. You've want to write a small piece of server side code that gives a [group](https://resources.catalyze.io/#groups) group called 'registrationStaff' with an ID of ```07d5119b-b975-4633-8157-e95cf8787087``` access to retrieve files. This code should look like this:

```var request = new XMLHttpRequest();```

```request.open('POST', 'https://api.catalyze.io/v2/acl/core/files/07d5119b-b975-4633-8157-e95cf8787087');```

```request.setRequestHeader('X-Api-Key', '<YOUR API KEY HERE>'); request.setRequestHeader('Authorization', 'Bearer <YOUR SESSION TOKEN HERE>'); request.setRequestHeader('Accept', 'application/json');```

```request.onreadystatechange = function () { if (this.readyState === 4) { console.log('Status:', this.status); console.log('Headers:', this.getAllResponseHeaders()); console.log('Body:', this.responseText); } };```

```var body = {'permissions': ['retrieve']};```

```request.send(body);
```





