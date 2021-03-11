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

## Operations on Tables

## View/Edit Contact
