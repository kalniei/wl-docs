###### User Accounts

Admin View && Distributor view only.
Here admin can manage accounts.

## Find Account

The Search filters, that are filtering the Accounts Table

1. Email Address
2. First Name
3. Last Name
4. Company Name
5. Company Keyword
6. Status - dropdown, allowed values: **All**, **Active**, **Inactive**

Button Search will trigger request to Api: 

```
GET /v1/admin/users
```
with optional filters params: `filters[]=`

```
email: string; - operand "l"
firstName: string;  - operand "l"
lastName: string;  - operand "l"
companyName: string;  - operand "l"
companyKeyword: string;  - operand "l"
status: string; - operand "eq"
```

Example of created link: 

```
GET /v1/admin/users?filters[]=email,l,test@lim.bz,&filters[]=firstName,l,testFirst,&filters[]=lastName,l,testLast,&filters[]=companyName,l,testInc,&filters[]=companyKeyword,l,banana,&filters[]=status,eq,active
```
***

## Accounts Table


Paginated list of all users, created under current Admin user.

We refresh Accounts table every **20** seconds.

By default pagination is set to **10** records.

All columns are sortable.

No columns have filters inside the table.

We use this route for getting accounts:

```
GET /v1/admin/users
```

Optional parameter for pagination:

```
  page: number;
  itemsPerPage: number; - 5 | 10 | 15
  showAllRecords: boolean;
  sortBy: string;
  sortDesc: boolean;
```

Example of created link: 

```
GET /v1/admin/users?page=1&itemsPerPage=10&showAllRecords=0&sortBy=lastName&sortDesc=1
```

Expected output:

```
{
  data: [
    {
      companyKeyword: "test"
      companyName: "Test Inc"
      email: "test@lim.bz"
      fbBusinessPage: 1111
      instagramId: 1111
      firstName: "Test"
      id: 1
      lastName: "Test"
      status: "active"
    }
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
    sortBy: "lastName"
    sortDesc: true
    totalItems: 463
    totalPages: 47
  }
}
```

### Impersonate User

Option to log in into users account.

```
GET /v1/users/${id}/?permissions=1
```

After successful response we do all actions like login user.

see: [user login](../account/login.md) for more details

Note - in Store we keep `parentUserId" ${adminId}` and  in Local Storage - `impersonate: ${adminEmail}`.

It allows us to go back to admin (same actions as impersonate - just use id in Store)

### Edit Account

On Edit Account the modal with account profile info is being opened.

Api for getting full User Object: 

```
GET /v1/users/${id}
```

Expected output:

```
{
  activeCampaigns: 0
  address: "NA"
  address1: null
  avatarUrl: null
  businessTypes: [{
    id: 8,
    name: "Retail Stores & Clubs"
  }]
  city: "Stamford"
  companyLogoUrl: "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/users/48/logo/7747763fbdc3f595adb3cfe5fd4d5ffcd093d3b8.png"
  companyName: "Bab"
  defaultAdCategory: "regular"
  domains: [{
      id: 55,
      domain: "30.d.lnx.bz",
      shorteners: [
        "https:\/\/30.d.lnx.bz\/rffef",
        "https:\/\/30.d.lnx.bz\/defmem-48",
        "https:\/\/30.d.lnx.bz\/cd9"
      ]
    }],
  email: "as@lim.bz"
  enabledFbPageAccess: false
  enabledInstagramAccess: false
  fbBusinessPage: null
  findCampaignId: 3257
  firstLogin: false
  firstName: "Half"
  franchise: null
  id: 48
  instagramId: null
  lastName: "Café"
  leaderboardCampaignId: null
  logoAspectRatio: "companyLogo"
  logoBgColor: "#ffffff"
  parent: null
  phone: "(111) 111-1111"
  plan: {
    tag: "paid",
    slug: "default-paid",
    startsAt: "2020-11-04T13:18:43+0000",
    endsAt: "2020-12-04T13:18:43+0000"
  }
  roles: ["ROLE_USER"]
  socialMedias: {
    twitter: null,
    facebook: null,
    linkedin: null,
    instagram: null
  }
  state: "CT"
  tcAccepted: true
  website: null
  zip: "06901"
}
```

Here we have three Admin ONLY settings:

1. *"Use this email address for ad campaign testing"*

    If switch is set to **true** - the property of User object `testingEmail` is set to the same value as field `email`.

    If switch is set to **false** - the new input becomes visible "Email address for ad testing" and property of User object is `testingEmail`.

2. *"Enable Access to Facebook Followers"*

    Default - **false**.

    If switch is set to **true** - this specific user will have a "Facebook Followers" option for targeting contacts in Campaign Builder.

    Property od User Object: `enabledFbPageAccess`.

    Please note: **Admin is responsible for consciously setting this flag ONLY when a specific user has `fbBusinessPage` id set.**

3. *"Enable Access to Instagram Followers"*

    Default - **false**.

    If switch is set to **true** - this specific user will have a "Instagram Followers" option for targeting contacts in Campaign Builder.

    Property od User Object: `enabledInstagramAccess`.
  
    Please note: **Admin is responsible for consciously setting this flag ONLY when specific user has `instagramId` set.**


Those, and all additional fields will be send in the object by Api:

```
PUT /v1/users/${id}
```

Expected output: full User object

#### Change Password
Admin can also change password of its users. The current password is not required.

The route for Admins change of password:

```
POST /v1/admin/users/${id}/changePassword
```

As parameter we send: 
```
newPassword: "12345678"
```

Expected output:

```
{
  password_changed: [true]
}
```

### QR Codes

Click on icon "QR codes" triggers open the modal "QR CODES FOR CAMPAIGNS"

Here we are asking Api to give us all users campaign with types **find** and **leaderboard**
(SMS campaign and Push Your Limits)

```
GET /v1/admin/users/${id}/campaigns?filters[]=type,in,find,leaderboard&page=1&itemsPerPage=3&showAllRecords=0
```

Expected output:

```
{
  data: [
    {
      adBuilder: []
      contactListIds: []
      created_at: "2020-11-03T13:15:09+0000"
      customLink: null
      emailTemplate: []
      endDate: null
      footerText: null
      gameUrl: "https://2E.d.lnx.bz/thyhyh"
      geoCity: null
      geoLat: null
      geoLong: null
      geoRadius: null
      geoState: null
      geoZip: null
      id: 3255
      isProcessingContacts: false
      keyword: "thyhyh"
      logoUrl: null
      managerAvatar: null
      maxCostPerDay: 0
      name: "SMS Campaign"
      peopleTargetingQty: null
      prizeTypes: []
      selectedGames: []
      startDate: "2020-11-03T13:15:02+0000"
      status: "active"
      targetAllContacts: false
      targetingType: "none"
      totalContacts: 0
      type: "find"
      updated_at: "2020-11-03T13:15:13+0000"
      useDefaultPrizes: true
    }
  ]
  state: {
    fields: []
    filterBy: null
    filterValue: null
    filters: [
      "type,in,find,leaderboard"
    ]
    itemsCount: 2
    itemsPerPage: 3
    joins: []
    page: 1
    showAllRecords: false
    sortBy: null
    sortDesc: false
    totalItems: 2
    totalPages: 1
  }
}
```

From Campaign Object in this View we only use **name**, **type** and **gameUrl**

We use component "QrCodeGenerator" for creating QR code for campaign **gameUrl**


### Delete Account

Click "Delete Account" icon will trigger conformation modal.

Once you confirm your action the delete request is being sent:

```
DELETE /v1/admin/users/${id}
```

Expected output: 

```
{
  data: null
  messages: ["User Test Test (test@lifeinmobile.com) has been successfully deleted."]
}
```