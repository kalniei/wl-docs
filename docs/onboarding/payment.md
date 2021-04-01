# Payment

Availability of this section is being managed through `onboarding.billing.view` in the chosen plan. On creating franchise we choose which plan should be a default plan, and id it is paid one - we should set onboarding billing to `true`

At the real begining we are sending two requests to obtain information about user's subscription. It is needed because onboarding process can be interrupted by user.

First we check does user has any saved credit cards

```
GET /v1/users/${userID}/cards
```

Expected output:

```
[
  {
    brand: "visa"
    country: "US"
    expMonth: 4
    expYear: 2024
    last4: "4242"
    paymentMethodId: "pm_1HJg1xDTu2PET25E8Z8oOxpE"
    threeDS: true
  }
]
```

If there is no credit card saved - on "Save" we first need to save credit card and get its `paymentMethodId`:

We do so useing stripe plugin inner function:

```
POST https://api.stripe.com/v1/payment_methods
```

After that we can activate subscription.

It is required to provide plan slug or ID to which Plan user is willing to subscribe.

We do not send request to get available plant for the user - we are getting it form Store. On login we recieve available plans and save them to Store.

```
POST /v1/stripe/subscribe

```

Request JSON body:

```
{
    paymentMethodId?: "pm_xxxxxxxxxxxx",
    plan: "my-plan-slug"
    code?: "my-promo-code"
}
```

If there is `code` param - `paymentMethodId` is not required and the other way around.

Expected output: 

```
{
  plan: { <wholePlanObject>}
  subscription: {
    status: "active" | "trialing"
  }
}
```