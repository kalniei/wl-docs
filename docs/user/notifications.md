# Notifications

We send Notification request on user login.

```
GET /v1/notifications
```

Expected output:

```
[
  {
    body: "There is an issue with your Win Local Campaign."
    createdAt: "2019-07-26T12:48:15+0000"
    id: 5
    status: "new"
  }
]
```

If there are any notifications we show notification Icon near User Icon at the top right corner on each page header.

Click on this icon will get us to Notification page.

We are able to archive chosen notification by sending:

```
PUT /v1/notifications/${notificationId}

```

with JSON body:
```
{
  status: 'archived'
}
```

At this moment as far as I know notifications are turned off. Need to bring it back.