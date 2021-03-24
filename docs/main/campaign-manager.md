# Campaign Manager

In this view we can see all campaigns, added by user.


Endpoint for campaigns table: 

```
GET /v1/campaigns?stats=basic
```

This endpoint accept all pagination, sorting and filtration rules available.

Expected outcome:

```
{
  data: [
    {
      adBuilder: []
      amount: 10000
      contactListIds: []
      created_at: "2021-03-22T09:57:53+0000"
      customLink: null
      emailTemplate: []
      endDate: "2021-04-10T21:59:59+0000"
      footerText: null
      gameUrl: "https://205.d.lnx.bz/d69"
      geoCity: null
      geoLat: null
      geoLong: null
      geoRadius: null
      geoState: null
      geoZip: null
      id: 3433
      isProcessingContacts: false
      keyword: null
      logoUrl: null
      managerAvatar: null
      maxCostPerDay: 1111
      name: "test end date"
      peopleTargetingQty: 0
      priceType: "fixed"
      prizeTypes: []
      selectedGames: []
      startDate: "2021-03-31T22:00:00+0000"
      stats: {
        basic: {
          clientPrizeRewards: {
            accepted: "0"
            redeemed: "0"
          }
          newEmails: 0
          newPhones: 0
          noOfContacts: 0
          submits: 0
          views: 0
          wlPrizeRewards: {
            accepted: "0"
            redeemed: "0"
          }
        }
      }
      status: "draft"
      targetAllContacts: true
      targetingType: "contacts"
      totalContacts: 0
      type: "social"
      updated_at: "2021-03-22T13:07:28+0000"
      useDefaultPrizes: true
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
    sortBy: null
    sortDesc: false
    totalItems: 55
    totalPages: 6
  }
}
```

Optional query parameter for pagination:

```
  page: number;
  itemsPerPage: number; - 5 | 10 | 15
  showAllRecords: boolean;
  sortBy: string;
  sortDesc: boolean;
```

We refresh campaigns reporting table every **20** seconds.

By default pagination is set to **10** records.

By default this table is neither sorted nor filtered.

Campaign Table headers:

```
  **Campaign Name**: 'name'
  **Campaign Type**: 'type'
  **Status**: 'status'
  **Date Created**: 'created_at'
  **End Date**: 'endDate'
  **Active PPL**: 'noOfContacts'
  **New Contacts**: 'newContacts'
```

We do not have ( at least now ) filters and sorting options in this table on fornt end side. Though we can filter campaigns by name adding this filter: `filters[]: name,l,test`.

In each row we have two actions available: edit campaign and show Report.

## Campaign Report

On Report click we show Single Campaign Report in a popup.

```
GET /v1/campaigns/${campaignId}?stats=all
```

Expected outcome - full campaign object with stats.

And in a popup we show the report modal of the campaign.


## Edit Campaign

