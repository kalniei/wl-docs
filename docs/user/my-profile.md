###### My Profile

My profile section could be reached from two places in portal: Side nav panel and top right Account Icon.

My profile Section has four main sections.

## General Info

**NOTE:** We do not send request to display general info of the user - we are getting it form Store. On login we recieve user's information and save it to Store.


Available fields to change in this section:


1. Email address for ad testing: `testingEmail: string`

2. First Name: `firstName: string`

3. Last Name: `lastName: string`

3. Company Name: `companyName: string`

5. 
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