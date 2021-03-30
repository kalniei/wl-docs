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
      useDefaultPrizes: tr`

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

There is no point of prezenting campaign object, cause properties might be different, depending on campaign type.

And in a popup we show the report modal of the campaign.

Depending on type of the campaign we see different things in the popup.

**SOCIAL Campaign**

On the left part there is a preview of the Facebook ( as default ) ad. We can switch to Instagram preview.

On the right part we can see statistics such as:

- Reach: `reach`

- Impressions: `impressions`

- New Leads: `new_leads`

- Verified Leads: `verfied_leads`

- Total Clicks: `total_clicks`

- Unique Clicks: `unique_clicks`

- Shares: `shares`

- Comments: `comments`

- Likes: `likes`

In the Prize tab we show either custom prize, added by user, or notification that this campaign does not have givaways, in case it is redirected to custom link or another campaign.

**EMAIL Campaign**

On the left part there is a preview of the Email Template that is sent to contacts.

On the right part we can see statistics such as:

- Opens: `opens`

- Total Clicks: `total_clicks`

- Unique Clicks: `unique_clicks`

- New Verified Email: `new_verfied_email`

- Unsubscribed: `unsubscribed`

- Bounced / Failed: `failed`

In the Prize tab we show either custom prize, added by user, or notification that this campaign does not have givaways, in case it is redirected to custom link or another campaign.



**FIND Campaign**

On the left part there is a preview of Game, that contacts will play by entering promoted short link.

On the right part we can see statistics such as:

- New Leads: `new_leads`

- Total SMS: `total_sms`


In the Prize tab we show either custom prize, added by user, or defaut prize, if not added any.


In all types of campaign we have an oppotrunitey to download grafical view of the report modal in .psd file.

It is front end operation, but we do use `${API_URL}/media/proxy?url=` link for loading images in the download.

## Edit Campaign

There are permissions to Edit campaign.

We can **ALWAYS** edit Find and PYL campaign.

At this moment all other campaigns can be edititd **ONLY** when their status is draft.

Though we have a task to edit Email Campaign (only change step 2 & 3 ) untill the emails are not send. After that - no editing possible, and Social Ad Campaign (only change step 2 & 3 ) all the time.+ hide checkout & buttons
+ think about informing user that the Evocalize will need some time to approve


When we edit PYL campaugn - we are redirected to PYL campaign subpage. In any other cases - we are redirected to Campaign Builder. 

At the side navigation pannel we can see all the steps that are required for chosen type of the campaign.

We CAN navigate through them, and also there is an option to go forward and backword inside each step.

**Please NOTE** if we are navigatinf through side panel - changes will not be saved. You need to press "Next" or "Save" ( depending on the step you are in right now ) in order to save your changes.


On edit campaign we are sending

```
GET /v1/campaigns/${idCampaign}
```

Expected output - whole campaign object.

There some more requests that we sending:


1) Destinations of the campaign:

```
GET /v1/campaigns/${ id }/destinations
```

Expected output:

```
[
  {
    campaign: 3434
    createdAt: "2021-03-21T23:00:00+0000"
    destinationCampaignId: null | 123
    endsAt: "2021-03-21T23:00:00+0000"
    id: 214
    startsAt: "2021-03-21T23:00:00+0000"
    type: "customLink" | "campaignDestination" | "game"
    updatedAt: "2021-03-21T23:00:00+0000"
    url: null | "https://test.pl"
  }
]
```

2) Prizes of the campaign:


```
GET /v1/campaigns/${ id }/prizetypes
```

Expected output: `[]` if destination is not `game` OR

```
[
  {
    amount: null
    endDate: null
    id: 238
    imageUrl: "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/campaigns/3434/prizetype/a40f546635be209ac9e36bef166385a1b906c540.png"
    name: "test"
  }
]
```


3) Social Ad if needed:

```
GET /v1/adbuilder/${ id }
```

Expected output:

```
{
  adType: "single image"
  advertStatus: "inactive"
  bodyText: "test"
  campaignId: "3434"
  headlineText: "test"
  images: [
    "https://images.unsplash.com/photo-1524580328839-a67fcb31f12a?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=Mnw2NTU1M3wwfDF8c2VhcmNofDIwfHxhbGx8ZW58MHx8fHwxNjE3MTE2OTgz&ixlib=rb-1.2.1&q=80&w=1080"
  ]
  linkDescription: null
  placement: "0"
  url: "https://contact.winlocal.xyz/offers/3434"
  videoId: ""
}
```

4) Email Ad if needed:

```
GET /v1/emailtemplate/${ id }
```

Expected output:

```
{
  body: "test"
  bodySecond: ""
  campaignId: "3105"
  headline: "test"
  headlineSecond: ""
  imageLogo: null
  imageMain: "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/campaigns/3105/ads/89dd0deae1f03de2e082123ca3864cdbf9f5c50d.png"
  imageSecond: null
  layoutId: "1"
  linkLabel: "test"
  previewText: "teet"
  startDate: "2021-07-08T15:29:00+0000"
  subject: "Test"
  url: "https://205.d.lnx.bz/c21"
}
```