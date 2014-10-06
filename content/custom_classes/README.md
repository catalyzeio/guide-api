# Building Custom Classes #

##Introduction##

Custom classes are the swiss army knife of Catalyze's secure data offerings on our Backend as a Service (BaaS). A limitless key/value storage system, custom classes can be used to quickly build any data models you need for your application and store data to those models, all in simple JSON. 

Custom classes can be intimidating at first because they support very flexible use cases. Our goal is to walk you through creating some practical use cases for custom classes in healthcare situations. 

**Pre-requisites**

To get through this and other Catalyze guides you will need the following:

- Developer Account
    * If you haven't already, sign up for a Catalyze [Developer account](https://dashboard.catalyze.io). Make sure to activate your account and successfully log in to the [Dashboard](https://dashboard.catalyze.io) before continuing.
    * You'll also want to have gone through [Getting Started](https://docs.catalyze.io/guides/api/latest/getting_started/README.html), at least to the point where  you have an application, api keys and know how to generate a session token with your login credentials. Want to go faster? [Follow the Instructions Here]()(https://docs.catalyze.io/api/latest/#sign-in)
- A terminal that can run **curl** (bundled with OS X, most Linux, Windows via cygwin) or have the Catalyze javascript SDK installed.

##Getting Started##

You are a software developer building a new app called _DocClock_. _DocClock_ allows users to find doctors online, see their available appointments, and schedule and pay with a credit card online. Since patients might enter some notes about their visit that are sensitive, it is important that you store this information in a way that is secure and HIPAA-compliant.

##Create Your Schema##

The first thing to do when creating a custom class is to determine what type of data you want to store and what you want to name the data. Right now, *DocClock* only needs one custom class: TimeSlots. A TimeSlot has the following data fields with corresponding data types:

`{
	"name" : "TimeSlot",
	"schema": {
	"date":"string",
	"startTime":"string",
	"endTime": "string",
	"length": integer,
	"visitType": "string",
	"provider": "string",
	"open": "boolean",
	"patient":"string"
	}
}`

The timeslot is a pretty basic object, but at its core, this is what your average scheduling application looks like. You'll notice that the most common data type in use is a string. It's the most common Custom Class data type. You'll notice a few other data types, like integer and boolean. These, along with "double" and "object", round out your Custom Class data types. Your data types, like SQL, provide some basic type checking/sanity checking for incoming data. 

We've built our Schema. Let's post it to Catalyze!

```curl -H "X-Api-Key: <Your Key>" -H "Authorization: Bearer <Your Token>" -H "Content-Type: application/json" https://api.catalyze.io/v2/classes -X POST -d '{"name":"TimeSlot”,”phi":false,"editable":true,"schema":{"date":"string","startTime":"string","endTime":"string","length":"integer","visitType":"string","provider":"string","open":"boolean","patient":"string”}}’
```

##Post entries##

Now we have the only data model we need to get started with our app. With our "database" up and running, let's post some sample data to the API.

The route that you will post to is:

```POST https://api.catalyze.io/v2/classes/TimeSlot/entry```

And the data that you will post is:

```curl -H "X-Api-Key: <Your Key>" -H "Authorization: Bearer <Your token>" -H "Content-Type: application/json" https://api.catalyze.io/v2/classes/TimeSlot/entry -X POST -d ‘{
"date":"08/01/2014",
"startTime":"13:00",
"endTime": "14:00",
"length": 60
"visitType": "Annual Check-up",
"provider": "provider_id",
"open": "true",
"patient":"patient_id"
}’```

##Query Custom Classes##

Did it work? Super. Now, if we want to retrieve our data, we'll need to write a query to do so. Let's write a query that grabs all TimeSlots that are open (open == true). 

Our route for Custom Class queries is: 

```GET https://api.catalyze.io/v2/classes/TimeSlot/query?<your query>```

You'll notice that our query is expressed through URL parameters. There are 4 parameters you can use for queries:

- field: The field you are looking for a value in
- searchBy: The value you are searching for
- pageSize (optional): The quantity of results you want returned at one time. The default is 10.
- page (optional): The page of results you want to retrive.

So, our query for a custom class looks like this


```curl -H "X-Api-Key: <Your Key>" -H "Authorization: Bearer <Your token>" -H "Content-Type: application/json" https://api.catalyze.io/v2/classes/TimeSlot/query?field=open&searchBy=true -X GET```

And we retrieve all open appointments stored in TimeSlots. Boom! DocClock is ready to go-live.






