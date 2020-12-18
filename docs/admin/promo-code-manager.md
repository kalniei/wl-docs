###### Promo Code Manager

Admin View only.
Here admin can manage subscription coupons.

Coupons can be used for activation of subscription without entering credit card. Coupons can not be used for paying for the campaign.

## Add Coupon

Request for creating new coupon:

```
POST /v1/coupons
```

Request JSON body:

```
{
  "type": multi_use | one_use,
  "value": number,
  "valueType": percentage | currency | days,
  "code": string | undefined,
}
```

**CODE**: required_if: type=multi_use otherwise is generate


## Coupons Table

Paginated list of all coupons. Pagination comes from the backend side.

We refresh Coupons table every 20 seconds.

By default pagination is set to **10** records.

No columns are sortable.

No columns has filters inside the table.

```
GET /v1/coupons
```

Avaliable JSON params:

```
{
  page: 1
  itemsPerPage: 10
  showAllRecords: 0 | 1
}

```

Expected output:

```
{
  data: [
    {
      code: "WL30PROMO"
      id: 1
      type: "multi_use"
      userCoupons: []
      value: 30
      valueType: "days"
    }
  ]
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
    totalItems: 12
    totalPages: 2
  }
}

```


We do have additional ability to assign coupon to user, but it is not used now.

```
POST /v1/users/{userId}/coupons/{couponId}
```


### Delete Coupon

Click "Delete Account" icon will trigger conformation modal.

Note: coupon will be deleted with user associations.

Once you confirm your action the delete request is being sent:

```
DELETE /v1/coupons/{id}
```

Expected output: 

```
{
  status: true
}
```