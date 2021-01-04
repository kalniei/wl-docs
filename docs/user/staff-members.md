###### Staff Members

Last known staff members mock up - [invisionapp.](https://projects.invisionapp.com/share/SVUOPET2KYG#/)
It gets general idea on how it should work, but design is a little bit changed.

## Add staff Member

Here we can create a short link for each employee. It should be used for promoting business via social post and sms messaging. You can choose a pin code for each staff member which will be used when a customer redeems an offer.

In Staff membere creation form you can also create short link by chosing domain and alias. More about creating short links [here](short-link.md)

Pin code must be 4 characters.

Route for creating staff member:

```
POST /v1/staff-members
```

JSON body:

```
{
  active: true | false
  domain: "205.d.lnx.bz"
  firstName: "test"
  jobDescription: "test"
  lastName: "test"
  pin: "1234"
  shortLink: "testtesttest"
  status: active | inactive
}
```

Expected output: newly created Staff Member object

The **status** switch is responsible for `active` boolean parameter.

At this moment no matter what we always `status: active`.

I guess backend is switching between `status: active | inactive` based on **active** field. We should cocider changing it.
## Staff Members Table

Paginated list of all staff members.

We refresh staff members table every **20** seconds.

By default pagination is set to **10** records.

All columns ARE NOT sortable at this time. ( though it would be nice to have this opportunity)

ALL columns have filters inside the table BUT only first and last name is actually working at the backend side. We need to fix it.

We use this route for getting staff members:

```
GET /v1/staff-members
```

Optional parameters:

```
  with_deleted: 0 | 1 - by default always 0
  dateTo: moment(chosenCustomDate).format('YYYY-MM-DD')
  dateFrom: moment(chosenCustomDate).format('YYYY-MM-DD')
```

**dateTo** and **dateFrom** option is available as a separated filter above the table.

**NOTE** AT THIS MOMENT FILTERING IS WORKING INCORRECT - it filters by creation date of staff member. BUT it should filter statistics of each staff member.

Optional parameter for pagination:

```
  page: number;
  itemsPerPage: number; - 5 | 10 | 15
  showAllRecords: boolean;
```

Optional filter params: `filters[]=`

```
  firstName: string;  - operand "l"
  lastName: string;  - operand "l"
  instagram: string;  - operand "l"
  status: string;  - operand "l"
  url: string;  - operand "l"
  facebook: string;  - operand "l"
  leads: string;  - operand "l"
  rdms: string;  - operand "l"
  www: string;  - operand "l"
```

Expected output:

```
{
  data: [
    {
      createdAt: "2021-01-04T11:12:57+00:00"
      firstName: "Test"
      id: 306
      isDeleted: false
      jobDescription: "test"
      lastName: "test"
      pin: "8787"
      redeemedPrizesCount: 0
      shortener: [
        {
          alias: "test2021"
          campaign: {id: 3355, name: "SMS Campaign", url: "https://256.d.lnx.bz/2021"}
          createdAt: "2021-01-04T11:12:57+0000"
          description: "Default staff member shortlink"
          domain: "256.d.lnx.bz"
          id: 2280
          imageTitle: null
          imageUrl: null
          stats: {
            facebook: 0,
            instagram: 0,
            sms: 0,
            www: 0,
            leads: 0,
            rdms: 0
          }
        }
      ]
      status: "inactive"
    },
    ...
  ],
  state: {
    fields: []
    filterBy: null
    filterValue: null
    filters: []
    itemsCount: 10
    itemsPerPage: 10
    joins: []
    page: 1
    showAllRecords: false
    sortBy: null
    sortDesc: false
    totalItems: 45
    totalPages: 5
  }
}
```

Each row represents one staff member. Clicking on each row you will be able to edit staff member.

## Table Actions

Table actions available above the table.

### Deactivate

We can choose all staff members or choose individually by checkbox at each row. This action will change the status of staff member to `inactive`.

**NOTE** if you choose staff member with status already `inactive` - you will not be able to proceed.

```
POST /v1/staff-members/deactivate
```

JSON body:

```
ids: [ 1, 2 ]
```

Expected output - array with changed short links objects.

### Delete
We can choose all staff members or choose individually by checkbox at each row.

```
POST /v1/staff-members/delete
```

JSON body:

```
ids: [ 1, 2 ]
```

Expected output:

```
[
  {
    deleted: true
    id: 1
  },
  {
    deleted: true
    id: 2
  }
]
```
### Reactivate

We can choose all staff members or choose individually by checkbox at each row. This action will change the status of staff member to `active`.

**NOTE** if you choose staff member with status already `active` - you will not be able to proceed.

```
POST /v1/staff-members/activate
```

JSON body:

```
ids: [ 1, 2 ]
```

Expected output - array with changed short links objects.
### Search
At this moment Search input is not doing anything. And we don't know what should it do.
### Filter by date
For filters we use optional query parameters:

```
  dateTo: moment(chosenCustomDate).format('YYYY-MM-DD')
  dateFrom: moment(chosenCustomDate).format('YYYY-MM-DD')
```

**dateFrom** - at this moment set to 15 days past.

**NOTE** AT THIS MOMENT FILTERING IS WORKING INCORRECT - it filters by creation date of staff member. BUT it should filter statistics of each staff member.

## Assign destination/campaign

Here we can assign to each Staff Member specific campaign link for traffic.

**I don't really know what is it about - will be clarified later**

Single select field. Here we choose which campaign will be assigned.

If no Staff member is chosen - by default we will assign chosen campaign to ALL staff members.

We can choose all staff members or choose individually by checkbox at each row. 

For the options we send request to get all users campaigns:

```
GET /v1/campaigns?page=1&showAllRecords=1
```

It will return all campaigns of user. After selections we get `campaignId` as a property of new Short Link.

For assigning campaign to staff member:

```
POST /v1/staff-members/destination
```

JSON body

```
  {
    campaignId: number
    ids: [1, 2]
  }
```

Expected output: `{}` - maybe we should change it.

## Edit Staff Member

By click on each row the Edit modal will pop up.

we have all options as in the (Add Staff member)[#add-staff-member]

The route for editinf staff member:

```
PUT /v1/staff-members/staffMemberId
```