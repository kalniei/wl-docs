###### login


Detailed info about User assigned Plan and permissions (by adding extra query param) you can get on standard Get user Details endpoint:

```
GET /v1/users/{id}?permissions=1

Example JSON output:
{
    "id": 3,
    "email": "rolibezfasoli@lim.bz",
    "phone": "(111) 111-1111",
    "roles": [
        "ROLE_USER"
    ],
    "firstName": "Roli",
    "lastName": "Bezfasoli",
    "companyName": null,
    "website": null,
    "address": null,
    "address1": null,
    "city": null,
    "state": null,
    "zip": null,
    "avatarUrl": null,
    "companyLogoUrl": "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/users/3/logo/3c6ff1b8ad55c495605f08f996c62b11a60c7a06.png",
    "fbBusinessPage": null,
    "socialMedias": {
        "twitter": null,
        "facebook": null,
        "linkedin": null,
        "instagram": null
    },
    "businessTypes": [],
    "activeCampaigns": 0,
    "parent": null,
    "firstLogin": false,
    "findCampaignId": 1,
    "tcAccepted": true,
    "plan": {
        "tag": "free",
        "slug": "default-free",
        "startsAt": "2020-05-08T10:41:53+0000",
        "endsAt": null
    },
    "permissions": [
        {
            "id": 1,
            "tag": "campaign.find.view"
        },
        {
            "id": 2,
            "tag": "campaign.find.create"
        },
        {
            "id": 3,
            "tag": "campaign.find.edit"
        },
        {
            "id": 4,
            "tag": "campaign.find.delete"
        },
        {
            "id": 21,
            "tag": "campaign.leaderboard.view"
        },
        {
            "id": 22,
            "tag": "campaign.leaderboard.create"
        },
        {
            "id": 23,
            "tag": "campaign.leaderboard.edit"
        },
        {
            "id": 24,
            "tag": "campaign.leaderboard.delete"
        }
    ]
} 
```