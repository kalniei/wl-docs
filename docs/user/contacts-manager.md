###### Contacts Manager

CONTACT DATABASE
Create, upload, and download contacts lists.


## All contacts Table

All Contacts table should NOT all contacts of the user.

it shows:

- imported contacts from Contact Manager

- collected contacts from SMS or PYL campaign

- collected contacs form Social or Email campaign

It MUST NOT show contacts, imported while creating the campaign.


Endpoint for all conatacts table: 

```
POST /v1/contacts-read
```

In this view we **always** use builder filter: `?filters[]=builder,eq,0` - this way we will not get contacts, imported in Campaign Builder


This endpoint accept all pagination, sorting and filtration rules available.

Expected outcome:

```
{
  data: [
    {
      address: null
      address1: null
      badges: null
      builder: 0
      city: null
      dob: null
      email: "olga.kalniei@gmail.com"
      engagements: 1
      firstName: "Olha"
      firstVisitDate: "2021-01-07T12:43:12+0000"
      gender: null
      id: 1408887
      lastName: "Kalniei"
      lastVisitDate: "2021-01-07T12:43:12+0000"
      list_ids: []
      phone: "14555555455"
      redemptions: 0
      source: "Direct"
      sourceCampaign: "SMS Campaign"
      state: null
      zip: null
    },
  ],
  state: {
    ffields: []
    filterBy: null
    filterValue: null
    filters: ["builder,eq,0"]
    itemsCount: 10
    itemsPerPage: 10
    joins: []
    page: 1
    showAllRecords: false
    sortBy: "lastVisitDate"
    sortDesc: true
    totalItems: 50197
    totalPages: 5020
  }
}
```

Optional query parameter for pagination:

```
  page: number;
  itemsPerPage: number; - 5 | 10 | 15
  showAllRecords: boolean;
  sortBy: string;
  sortDesc: boolean;
```

Additionally we can send one more filter: `filters[]: listIds,fis,${listId}`


Providing `listIds` will limit results to the contacts of this particular list.


We refresh contacts table every **20** seconds.

By default pagination is set to **10** records.

By default this table is sorted by `lastVisitDate` property - from the latest.

Contacts Table headers:

```
  **Mobile #**: phone
  **First Name**: firstName
  **Last Name**: lastName
  **Email**: email
  **Source**: source
  **Source Campaign**: sourceCampaign
  **Badges**: badges
  **Visit Date**: lastVisitDate
  **Visit time**: lastVisitDate
  **ENGS**: engagements
  **RDMS**: redemptions
```

## Contacts List Table

When we enter the Contact Manger view - we send a separated request for contacts lists:

```
GET /v1/contactlist
```

Expected output: 

```
[
  {
    id: 1
    name: "list test"
  }
]
```

We refresh contacts lists every **20** seconds.


If we want to see contacts from chosen list - we are adding this filter: `filters[]: listIds,fis,226` to contact request.

we can delete contact list:

```
DELETE /v1/contactlist/${listId}
```

Output: `{}`


## Operations on Tables

Above the table there are four icons - available actions with contacts. 

We also have search above the contact table - at this moment it does nothing.

In the contacts table each row ( contact ) has checkbox at the left. Useing those checkboxes we can choose contacts, which we want perform actions.

There is also "Check all" opportunity, though it will only check contacts at this page in the table.

### Create/Add to List

Here you can either create new list or add contacts to existing one.

**Create New List and Add Contacts:**

```
POST /v1/contactlist
```

Request JSON body:

```
contactIds: [1, 2, 3]
name: "Test"
```

Expected output:

```
{
  id: 1
  name: "Test"
}
```

**Add Contacts to Existing List:**

```
POST /v1/contactlist/${listId}/add
```

Request JSON body:

```
contactIds: [1, 2, 3]
```

Expected output:

```
{
  id: 1
  name: "Test"
}
```

### Export Contacts

After chosing contacts we can export them into .csv file

This is strict front end operation - no backend needed.

In the table we use foloowing headers:

- id

- first_name

- last_name

- email

- phone

- lastVisitDate

- engagements

- redemptions


### Import Contacts

On Import click the new window will appear. 

**NOTE** we use the same component here and in the Campaign Builder. The difference is - in Campaign Builder we use as a name of the list - file name. Here - we ask user to give us a name of the list. It is required property.

Imported contacts will be added to the newly created list.

We can also download example of acceptable .csv - this is front end operation, no backend needed.


On Upload contacts to the preview - we will se beging of the file.

We can choose - if we want to use firts row as headers or no. 

Acceptable headers:

- first_name

- last_name

- email

- phone

It is not possible to upload .csv file if headers are invilid.

**Please note** there MUST be either email or phone field. If we do not use first row as a header - backend will look for email column.

Upload contacts:

```
POST /v1/contacts/upload/csv
```

Request JSON body:

```
{
  csv: (binary)
  firstRowHeader: 0 | 1
  contactListName: string
}
```

Expected output:

```
{
  contactListId: 578
  message: "File is uploaded and will be processed."
  possibleNumberOfContacts: 51882
}
```

It will start backend process of uploading contacts. Number of contacts might be different form `possibleNumberOfContacts` because of the errors in .csv file.


### Remove Contacts from the list

This option is only available when user is on the List view of the table.

After chosing contacts we want to remove form the list we send: 

```
POST /v1/contactlist/${listId}/remove
```

Request JSON body:

```
contactIds: [1, 2, 3]
```

Expected output:

```
{
  id: 1
  name: "Test"
}
```
## View/Edit Contact

Each row is clickable.

On row click - we will be redirected to a new view - Singel Contact View.

On click we ask for detailed information about this contact:

```
GET /v1/contact/${contactId}

```

Expected output: 

```
{
  "id": 2053,
  "firstName": "Olha",
  "lastName": "Kalniei",
  "email": "olga.kalniei@gmail.com",
  "phone": "1212121212",
  "source": "Direct",
  "sourceCampaign": "SMS Campaign",
  "badges": [],
  "lastVisitDate": null,
  "firstVisitDate": null,
  "engagements": 0,
  "redemptions": 0,
  "dob": {
    "date": "2016-05-11 00:00:00.000000",
    "timezone_type": 3,
    "timezone": "UTC"
  },
  "gender": "f",
  "city": "test",
  "state": "Single",
  "zip": "5556",
  "address": "test",
  "address1": null,
  "emails": [{
    "email": "olga.kalniei@gmail.com",
    "isUnsubscribed": false
  }, {
    "email": "olga.kalniei+5555@gmail.com",
    "isUnsubscribed": false
  }, {
    "email": "olga.kalniei+88888@gmail.com",
    "isUnsubscribed": false
  }, {
    "email": "olga.kalniei+8888888@gmail.com",
    "isUnsubscribed": false
  }, {
    "email": "olga.kalniei+lol@gmail.com",
    "isUnsubscribed": false
  }],
  "phones": [11111111111, 11212121212, 12242212121, 15454545454, 15564454545, 17865415515, 18888888888, 18999999999],
  "firstNames": ["Olha", "Olia Kalniei"],
  "lastNames": ["Kalniei"],
  "igJourneys": 0,
  "fbJourneys": 0,
  "smsJourneys": 0,
  "emailJourneys": 0,
  "totalJourneys": 23,
  "totalInteractions": 23
}
```

Contact info available for changing ( from front end side ):

- First name: `firstName`

- Last name: `lastName`

- Default email: `email`

- Default Mobile Number: `phone`

- Birth Date: `dob`

- Gender: `gender`

- Address: `address`

- City: `city`

- State: `state`

- Zip: `zip`


On save we send:

```
POST /v1/contact/${contactId}
```

Request JSON body - whole contacs object with changed data.
Expected output - whole contacs object with changed data.


After successful save we are getting redirected back to Contacts Manager 