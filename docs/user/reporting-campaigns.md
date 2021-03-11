###### Reporting Campaigns

Detailed review of campaign statistics.


Endpoint for reporting table: 

```
GET /v1/users/${userId}/campaigns/stats
```

This endpoint accept all pagination, sorting and filtration rules available.

Expected outcome:

```
{
  data: [
    {
      clicks: 12
      cpc: 1666.6666666666667
      cpi: 4.9504950495049505
      cpr: 12.845215157353886
      desktop: 0
      id: 836
      impressions: 4040
      leadsForm: 0
      mobile: 12
      name: "Looking for a job"
      preview: "https://wl-client-panel.s3.amazonaws.com/campaigns/836/ads/262b795908b4cf7cfc5afc31eb1c96d82c20e412.png"
      reach: 1557
      source: "facebook"
      totalSpent: 20000
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

Reporting Table headers:

```
  **Ad Thumbnail**: '',
  **Campaign Name**: name
  **Reach**: reach
  **Impressions**: impressions
  **Clicks**: clicks
  **Lead Form**: leadsForm
  **Source**: source
  **CPR**: cpr
  **CPI**: cpi
  **CPC**: cpc
  **Total Spent**: totalSpent
```

Available `source` falues and their icons:

```
  'facebook': Facebook,
  'instagram': Instagram,
  'facebook,instagram': Facebook,
  'facebook_audience': Audience,
```

Each row is clickable, we show Single Campaign Report in a popup.
For more information see [Campaign Manager](../../campaigns/campaign-manager)