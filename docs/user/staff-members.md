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
  active: true
  domain: "205.d.lnx.bz"
  firstName: "test"
  jobDescription: "test"
  lastName: "test"
  pin: "1234"
  shortLink: "testtesttest"
  status: "active"
}
```

Expected output: newly created Staff Member object
## Staff Members Table

## Table Actions

## Assign destination/campaign

## Edit Staff Member