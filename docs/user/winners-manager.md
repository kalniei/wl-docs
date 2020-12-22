# Winners Manager

Winners table shows all winners for all campaigns. We show winners even if they did not redeemed.

Endpoint for winners table: 

```
POST /v1/winners
```

This endpoint accept all pagination, sorting and filtration rules available.

Expected outcome:

```
{
  data: [
    {
      campaignId: 3157
      campaignLogo: null
      campaignName: "SMS Campaign"
      campaignType: "find"
      contactId: 874283
      firstName: "tom"
      lastName: "rol"
      prizeImage: "https://agency-bucket.s3.us-east-2.amazonaws.com/national-sweepstakes/dev/RH7pzfmtOo5HtxVP7pROwL6o7JeGp1APnNUruFOJ.jpeg"
      prizeName: "Premium Sound Blue Tooth Speaker"
      redeemedAt: "2020-12-09T14:02:52+0000"
      staffMember: null
      wonAt: "2020-12-09T13:58:03+0000"
    },
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
    sortBy: "wonAt"
    sortDesc: true
    totalItems: 23
    totalPages: 3
  }
}
```

Optional query params for data faltering:

```
from: YYYY-MM-DD
to: YYYY-MM-DD
```

Optional query parameter for pagination:

```
  page: number;
  itemsPerPage: number; - 5 | 10 | 15
  showAllRecords: boolean;
  sortBy: string;
  sortDesc: boolean;
```

Additionally we can send a few more optional attributes as part of JSON body as shown on example above.

Request JSON body:
```
{
    "contactId": 1992,
    "campaignIds": [233, 343, 123],
    "prizeNames": ["card", "gift", "Client super prize"]
}
```

Providing `contactId` will limit results to the prizes wone by selected contact ID.

Providing single integer or array of integers as `campaignId` will limit results to the selected campaign or campaings (for multiple selection).

Providing the prize name or names will limit results to selected prize or prizes (for multiple selection) ... please keep in mind that this attribute applies search as LIKE operator, so all results containing a searchable query will be returned.

At this point in portal we use three filters:

1. **Date** - simple date picker. Default - three past month.
2. **Campaign** - we send request to get all users campaigns:

    ```
    GET /v1/campaigns?page=1&showAllRecords=1
    ```
    It will return all campaigns of user. After selection in multiselect we create an array of selected campaign ids.

3. **Prize** - we send request to get all won prizes:

    ```
    GET /v1/winners/prizes
    ```

    As an outcome we expect an array with prize names. After selection in multiselect we create an array of selected prize names.

We refresh prizes and winners table every **20** seconds.

By default pagination is set to **10** records.

By default this table is sorted by `wonAt` property - date of winning, from the latest.

Winner Table headers:

```
   **First Name**: firstName
   **Last Name**: lastName
   **Prize Name**: prizeName
   **Campaign Name**: campaignName
   **Campaign Report**: <customIcon>
   **Win Date**: wonAt
   **Redeemed Date**: redeemedAt
   **Redeemed By**: staffMember
```

**Win Date** is clickable, we show Prize Preview in a popup.

**Campaign Report** is clickable - we send request for getting campaigns details of this winner:

```
GET /v1/campaigns/${campaignId}?stats=all
```
Expected outcome - full campaign object with stats.

And in a popup we show the report modal of the campaign.

For more see [Campaign Manager](../campaigns/campaign-manager.md)