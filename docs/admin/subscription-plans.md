###### Subscriptions Plans

Admin View only.
Here admin can manage subscription plans.

**Subscription plan** - it is mainly set of permissions and rules, that can be added to specific franchise in order to limit users abilities and/or get specific paid according to this abilities.

One franchise can have few subscription plans and those plans can be switched.

## Add Plan

Request for creating new plan:


```
POST /v1/plan
```

Request JSON body:
```
{
  tag: 'free' | 'paid' | 'unlimited',
  slug: string | undefined,
  stripeId: string | undefined,
  period: string | null,
  trialPeriodDays: number | undefined, <not used for now>
  period: 'month' | 'year' | null, <not used for now>
  price: this.planTag === 'paid' ? {
    amount: number,
    currency: 'USD',
    formatted: {
        decimal: number,
        money: '$' + number,
    },
  } : undefined,
  permissions: [number, number, number],
},
```

**SLUG** is an optional parameter - if not provided - will be generated base on plan's NAME. Although, it is an optional parameter, must be unique within all available plans.

**STRIPE ID** is required only if newly created plan is of TAG=paid and it must be a valid Stripe Plan ID received on plan registration for example: via Stripe Panel. Otherwise, it can be set to `null` or completely omitted from the payload.

**PERIOD** must be one of: `month`, `year` or `null`. It determines how long the plan will remain valid. For example all plans with TAGs `free` or `unlimited` will very likely require PERIOD=null while plans with TAG `paid` will require PERIOD=month for montly subscriptions or `year` for annual ones.

**PERMISSIONS** should be an array list of assigned permissions. It can be an array of permissions IDs or full permission objects (at least permission ID is mandatory).

Expected output - object with newly created plan:

```
{
  id: 20
  name: "Test Plan"
  period: null
  price: {
    amount: "0",
    currency: "USD",
    formatted: {
      decimal: "0",
      money: "$0.00"
    }
  }
  slug: "test"
  stripeId: null
  tag: "free"
  trialPeriodDays: 0
}
```

We get avaliable permissions form here:

```
GET /v1/permissions
```

JSON response with all avaliable permissions:

```
[{
  "id": 44,
  "tag": "account.main.view"
}, {
  "id": 45,
  "tag": "account.profile.view"
}, {
  "id": 47,
  "tag": "account.shortLinks.view"
}, {
  "id": 46,
  "tag": "account.subscription.view"
}, {
  "id": 42,
  "tag": "campaign.builder.view"
}, {
  "id": 56,
  "tag": "campaign.email.create"
}, {
  "id": 57,
  "tag": "campaign.email.delete"
}, {
  "id": 58,
  "tag": "campaign.email.edit"
}, {
  "id": 59,
  "tag": "campaign.email.view"
}, {
  "id": 60,
  "tag": "campaign.find.create"
}, {
  "id": 61,
  "tag": "campaign.find.delete"
}, {
  "id": 62,
  "tag": "campaign.find.edit"
}, {
  "id": 80,
  "tag": "campaign.find.multiple_keywords"
}, {
  "id": 63,
  "tag": "campaign.find.view"
}, {
  "id": 64,
  "tag": "campaign.leaderboard.create"
}, {
  "id": 65,
  "tag": "campaign.leaderboard.delete"
}, {
  "id": 66,
  "tag": "campaign.leaderboard.edit"
}, {
  "id": 67,
  "tag": "campaign.leaderboard.view"
}, {
  "id": 43,
  "tag": "campaign.manager.view"
}, {
  "id": 68,
  "tag": "campaign.mixed.create"
}, {
  "id": 69,
  "tag": "campaign.mixed.delete"
}, {
  "id": 70,
  "tag": "campaign.mixed.edit"
}, {
  "id": 71,
  "tag": "campaign.mixed.view"
}, {
  "id": 72,
  "tag": "campaign.sister.create"
}, {
  "id": 73,
  "tag": "campaign.sister.delete"
}, {
  "id": 74,
  "tag": "campaign.sister.edit"
}, {
  "id": 75,
  "tag": "campaign.sister.view"
}, {
  "id": 76,
  "tag": "campaign.social.create"
}, {
  "id": 77,
  "tag": "campaign.social.delete"
}, {
  "id": 78,
  "tag": "campaign.social.edit"
}, {
  "id": 79,
  "tag": "campaign.social.view"
}, {
  "id": 52,
  "tag": "contact.engagements.view"
}, {
  "id": 50,
  "tag": "contact.main.create"
}, {
  "id": 51,
  "tag": "contact.main.update"
}, {
  "id": 49,
  "tag": "contact.main.view"
}, {
  "id": 41,
  "tag": "dashboard.main.view"
}, {
  "id": 48,
  "tag": "onboarding.billing.view"
}, {
  "id": 54,
  "tag": "staffMember.main.create"
}, {
  "id": 53,
  "tag": "staffMember.main.view"
}, {
  "id": 55,
  "tag": "winner.main.view"
}]
```

***

## Plans Table

Plans table is a simple table without backend pagination and filtration options.

We refresh Plan table every 20 seconds.


To get list of all available plans:

```
GET /v1/plan
```

**Please note:** This endpoint will manage the results of the response. Users granted SUPER ADMIN role will get a full list of all available plans while users assigned to a Franchise will get a list of all assigned plans to their Franchise only, and other users will get a list of default plans only.

To get single Plan details:

```
GET /v1/plan/{idSlug}
```

**Please note:**

All of the endpoints above will accept extra query parameter (as URL query param):

- `permissions` -> if set to `1` will add a list of assigned permissions to the plan output;


Example of Plan Object response: 

(will be array of objects for all plans request)

```
{
    "id": 1,
    "tag": "free",
    "slug": "default-free",
    "name": "Plan FREE",
    "stripeId": null,
    "period": null,
    "trialPeriodDays": 0,
    "price": {
        "amount": "0",
        "currency": "USD",
        "formatted": {
            "decimal": "0.00",
            "money": "$0.00"
        }
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
    ] <optional>
}
```

### Edit Plan

Click on the "Edit plan" icon triggers open edit modal.

There is only a plan name and a list of permissions able to be edited.


```
PUT /v1/plan
```

Request JSON body:
```
{
    id: 1,
    name: "My Plan new name",
    permissions: [1,2]
}
```

Please note: Although the plan name may change, the slug generated at plan creation will always remain the same.

