###### My Profile

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