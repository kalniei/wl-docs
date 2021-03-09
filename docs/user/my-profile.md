###### My Profile

My profile section could be reached from two places in portal: Side nav panel and top right Account Icon.

My profile Section has four main sections.

## General Info

**NOTE:** We do not send request to display general info of the user - we are getting it form Store. On login we recieve user's information and save it to Store.

Example of user object: 

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

Available fields to change in this section:


- Email address for ad testing: `testingEmail: string`

- First Name: `firstName: string`

- Last Name: `lastName: string`

- Company Name: `companyName: string`

- Default Ad Category: `defaultAdCategory: number`. 
As value we send id of the category
```
{ title: 'Regular', id: 0 },
{ title: 'Housing', id: 1 },
```

- Business Type: `businessTypes: number[]`.
As element of the array we send id of chosen business type. At this moment it is single select.
We get list of available business types:
```
GET /v1/business-types
```
Expected output:

```
[
    {
        id: number
        name: string
    },
]
```

- Phone number: `phone: string`

- Website: `website: string`

- Address: `address: string`

- Address 2: `address1: string`

- Zip: `zip: string | number`

- State: `state: string`

- City: `city: string`

For filling in State, Zip and City on the front end side we use here.maps api for autocomplete input.

On "Save" we send request to 

```
PUT /v1/users/${userId}
```

In params we send whole user object. 

Expected output - updated user object. We save it to Store.

#### Change Password
In this section we also can **Change Password**. On click - the new modal with fields will open.

The current password is required.

The route for change of password:

```
POST /v1/users/${id}/changePassword
```

As parameter we send: 
```
oldPassword: "11111111"
newPassword: "12345678"
```

New password must be at least 8 characters long.

Expected output:

```
{
  password_changed: [true]
}
```


## Subscription

## Logo

## Social Media

On user subscription, it is required to provide plan slug or ID to which Plan user is willin to opt in:
```
POST /v1/stripe/subscribe

Request JSON body:
{
    "paymentMethodId": "pm_xxxxxxxxxxxx",
    "plan": "my-plan-slug"
}
```


Since now route POST /v1/stripe/subscribe allows to pass "code" parameter to use it, paymentMethodId is not required then.