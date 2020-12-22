###### Dashboard

The last known mockup: 
[invisionapp](https://projects.invisionapp.com/share/9ZUYTXQW7BG#/screens/394746887) 

## Filter Range

All Dashboard components have the same filter component: DateRangeFilter

Those are available options for filter: 
```
  [
    {
      title: 'Today',
      value: 'today',
      measure: 'hours',
      startDate: moment().format('YYYY-MM-DD'),
      endDate: moment().add(1, 'days').format('YYYY-MM-DD'),
    },
    {
      title: 'Yesterday',
      value: 'yesterday',
      measure: 'hours',
      startDate: moment().subtract(1, 'days').format('YYYY-MM-DD'),
      endDate: moment().format('YYYY-MM-DD'),
    },
    {
      title: 'Last 7 Days',
      value: '7_days',
      measure: 'weekday',
      startDate: moment().subtract(7, 'days').format('YYYY-MM-DD'),
      endDate: moment().format('YYYY-MM-DD'),
    },
    {
      title: 'This Month',
      value: 'this_month',
      measure: 'days',
      startDate: moment().startOf('month').format('YYYY-MM-DD'),
      endDate: moment().format('YYYY-MM-DD'),
    },
    {
      title: 'Last Month',
      value: 'last_month',
      measure: 'days',
      startDate: moment().subtract(1, 'months').startOf('month').format('YYYY-MM-DD'),
      endDate: moment().subtract(1, 'months').endOf('month').format('YYYY-MM-DD'),
    },
    {
      title: 'Custom Range',
      value: 'custom',
      measure: 'custom',
      startDate: moment(chosenCustomDate).format('YYYY-MM-DD'),
      endDate: moment(chosenCustomDate).format('YYYY-MM-DD'),
    },
  ];
```

Some explanation on data range logic: 

- **Today** -> today will be compared to yesterday;

- **Yesterday** -> yesterday will be compared to day before (today -2days to today -1day);

- **Last 7 days** -> period of last 7 days will be compared to the previous 7 days before this period (-14 days to -7days);

- **This month** -> from the beginning of this month compared to the previous month;

- **Last Month** -> previous month is being compared to the month before;

- **Custom range** -> x days calculated form the date range compared to the x days before the beginning of this range;


In Api request we use encoded format: `encodeURIComponent(moment(date).toISOString())`

## Stats

For all stats we use same params:
```
?endDate=${end}&startDate=${start}
```

In Stas boxes we also send additional param: `&selectedPeriod=${val.value}` - not sure it is needed

Stats boxes are filtered statistics that are displaying the increase or decrease of specific contacts in chosen period.

Expected output:
```
{
  basePeriod: "2020-12-01T00:00:00+0000 - 2020-12-21T23:59:59+0000"
  basePeriodCount: 50003
  comparablePeriod: "2020-11-01T00:00:00+0000 - 2020-11-30T23:59:59+0000"
  comparablePeriodCount: 64310
  comparablePeriodDiff: -0.22246928937956773
}
```


Attribute `comparablePeriodDiff` can provide both negativ or positive values. Should be multiplied by 100 to get proper % for instance: -1 * 100 = -100% or 1.345 * 100 = 123.5%

#### New Contacts
```dashboard/stats?context=new_contacts```

New Contacts - *newly added / filled the form on game platform* [NEED TO BE VERIFIED]

**NOTE:** We should not see increase of contact if contacts were uploaded through Campaign Builder
#### Verified Contacts

Verified Contacts - *contacts whose email or phone was verified. ( like clicking in email, sending sms tp.)* [NEED TO BE VERIFIED]

```dashboard/stats?context=verified_contacts```
#### Offers Redeemed
Offers Redeemed - *contacts that redeemed at least one prize* [NEED TO BE VERIFIED]


```dashboard/stats?context=offers_redeemed```
## Chart

Endpoint for chart date:
```
GET /v1/dashboard/chart?endDate=${end}&startDate=${start}
```

Backend sends agregated data via time frames.

Timeframes depend on date ranges send from frontend.

Possible time frames:

- **Quarter** (15min) - when date range is about 1 day

- **Hour** (1h) - when date range is greater than 1 days and lesser than week.

- **Day** (1d) - when date range is greater than week (7 days).

Expected output: 

```
{
  data: [
    {
      date: "2020-11-30T00:00:00+0000",
      values: {
        direct: 0
        email: 0
        facebook: 0
        instagram: 0
        sms: 0
      }
    }
  ],
  interval: "day"
}
```


## Top Staff

Endpoint for top staff:

```
GET /v1/dashboard/top-members?endDate=${end}&startDate=${start}
```

Expected output:
```
[
  {
    clicks: "0"
    first_name: "Jane"
    id: "231"
    last_name: "Blue"
    leads: "0"
    rdms: "0"
  }
]
```

Returns best staff members based on their shortlink and traffic due to it.

For now best member is the one with highest sum of clicks, redeems and leads

## Recent Contacts

At this point Recent contacts are using the same endpoint as in [Contacts Manager](contacts-manager.md).
For more information look there.

```
GET /v1/contacts-read?filters[]=builder,eq,0
```

By default we use filter `builder,eq,0` - if set to 0 we will not get contacts imported form Campaign Builder.

Start and End date form FilterRangeComponent is used as aditional filter: `filters[]=firstVisitDate,b,${start},${end}`

Columns are filterable and sortable, paginated from backend.

We refresh table every **20** seconds.

Click on each row will open Contact View; more info -[Contacts Manager](contacts-manager.md).

Needed headers for Recent Contacts on dashboard:
```
**Mobile #**: phone

**First Name**: firstName

**Last Name**: lastName

**Email**: email

**Source**: source

**Source Campaign**: sourceCampaign

**Badges**: badges

**Visit Date**: lastVisitDate

**Visit time**: lastVisitDate

**ENGS**: engagements

**RDMS**: redemptions
```