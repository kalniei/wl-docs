# User info

Here we can fill personal user info.
We are getting user object form Store and if user has some information - we autofill.

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


- First Name: `firstName: string`

- Last Name: `lastName: string`

- Business Name: `companyName: string`

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

- Acceptance of terms and conditions: `acceptTerms: boolean`

- Address: `address: string`

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