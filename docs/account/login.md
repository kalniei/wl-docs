# Login

At the entering Login Page we are checking if there is query param `f`. If there is not - we use defaul winlocal plan and view.

If we do have the query param, we send request to get franchise useing query param:

```
GET /v1/admin/franchise/${queryF}
```

Expected output:

```
{
    defaultPlan: "supportlocal-free"
    id: 1
    logoUrl: "https://supportlocal-stage.lim.bz/img/support_local.png"
    name: "SupportLocal"
    prefix: "sl"
    signUpImageUrl: "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/franchise/1/signup/game-screen1.png"
    slug: "supportlocal"
}
```


In this view we have two fields to fill: `email` and `password`.

On Login we send:

```
POST /v1/auth/login
```

Json body: 

```
{
    email: "test@gmail.com"
    password: "11111111"
}
```

Expected output:

404, if password or email is not correct

```
{
    message:    "Username could not be found." |
                "Invalid credentials." |
                "Need to confirm email address."
}
```

**NOTE** if we get *"Need to confirm email address."* - we will redirect user to [Confirm Email](confirmation-link.md) page.

200, if login is successful

```
{
    plan: "default-paid"
    token: tokenString
}
```


If login was successful we send following requests to obtain needed information:

1) Available plans for user:

```
GET /v1/plan
```

Expected output - array with plans.

```
[
    {
        id: 13
        name: "Plan FREE"
        period: null
        price: {
            amount: "0"
            currency: "USD"
            formatted: {
                decimal: "0"
                money: "$0.00"
            }
        }    
        slug: "default-free"
        stripeId: null
        tag: "free"
        trialPeriodDays: 0
    },
]
```

2) Information about current user subscription

```
GET /v1/stripe/subscription
```

Expected output:

200:

```
{
  canceledAt: null
  created: "2020-12-01T12:46:21+00:00"
  currentPeriodEnd: "2021-04-01T12:46:21+00:00"
  currentPeriodStart: "2021-03-01T12:46:21+00:00"
  endedAt: null
  expiresAt: null
  plan: {
    id: "membership_monthly"
    nickname: "Monthly Membership"
  }
  status: "active" | "expires"
  trialEnd: null
  trialStart: null
}
```

**OR**

403: 

```
{
  security: ["Action is denied!"]
}

```

3) Notification request:

```
GET /v1/notifications
```

Expected output:

```
[
  {
    body: "There is an issue with your Win Local Campaign."
    createdAt: "2019-07-26T12:48:15+0000"
    id: 5
    status: "new"
  }
]
```


4) Detailed info about User assigned Plan and permissions (by adding extra query param).

We get `userId` from token.

```
GET /v1/users/{userId}?permissions=1

```

Example JSON output:

```
{
    "id": 3,
    "email": "rolibezfasoli@lim.bz",
    "phone": "(111) 111-1111",
    "roles": [
        "ROLE_USER"
    ],
    "firstName": "Roli",
    "lastName": "Bezfasoli",
    "companyName": null,
    "website": null,
    "address": null,
    "address1": null,
    "city": null,
    "state": null,
    "zip": null,
    "avatarUrl": null,
    "companyLogoUrl": "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/users/3/logo/3c6ff1b8ad55c495605f08f996c62b11a60c7a06.png",
    "fbBusinessPage": null,
    "socialMedias": {
        "twitter": null,
        "facebook": null,
        "linkedin": null,
        "instagram": null
    },
    "businessTypes": [],
    "activeCampaigns": 0,
    "parent": null,
    "firstLogin": false,
    "findCampaignId": 1,
    "tcAccepted": true,
    "plan": {
        "tag": "free",
        "slug": "default-free",
        "startsAt": "2020-05-08T10:41:53+0000",
        "endsAt": null
    },
    "permissions": [
        {
            "id": 1,
            "tag": "campaign.find.view"
        },
        {
            "id": 2,
            "tag": "campaign.find.create"
        },
        {
            "id": 3,
            "tag": "campaign.find.edit"
        },
        {
            "id": 4,
            "tag": "campaign.find.delete"
        },
        {
            "id": 21,
            "tag": "campaign.leaderboard.view"
        },
        {
            "id": 22,
            "tag": "campaign.leaderboard.create"
        },
        {
            "id": 23,
            "tag": "campaign.leaderboard.edit"
        },
        {
            "id": 24,
            "tag": "campaign.leaderboard.delete"
        }
    ]
} 
```

**IF** user property `firstLogin` is set to **true** AND user is neither Admin not Distributor - we redirect to Onboarding.

**ELSE** - send request about subscription and redirect to Dashbard.