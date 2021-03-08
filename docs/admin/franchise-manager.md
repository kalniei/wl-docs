###### Franchise Manager

Admin View && Distributor view only.
Here admin can manage subscription Franchise.

**Franchise** - it is a wrapper for users with it's own plans and special functionalities.

Franchises get a signup link - all clients that register via  that link belong to that franchise. 
Accounts can be tracked by franchise

The Franchise Sign Up Link also has a default subscription plan.

## Add Franchise

When adding a new franchise we should :

 - get registration/login link for franchise (user registered by that link/form will be assigned to the particular franchise). The page will display fanchase logo instead of WinLocal logo and franchise supporting image at the left of the login/registration form. For example: *https://client.winlocal.xyz/?f=franchiseName*

 - choose a subscriptions plans availible for that franchise.

 - choose a default plan for new registered users.

 - add two letter as shortlink prefix ie. WL for shortlinks for WinLocal franchise (default franchise), SL for Support Local franchise etc.

To create a new franchise:
```
POST /v1/franchise
```

Request JSON body:

```
{
	"name": "My Franchise 3",
	"prefix": "m3",
	"defaultPlan": "free",
	"plans": [1,2,3]
} 
```

At the moment of franchise creation you need to provide a list of available Plan IDs or Plan SLUGs.

To get list of all available plans:

```
GET /v1/plan
```

**Franchise name** must be unique for each franchise - based on the franchise name there will be relevant slug generated.

**Prefix** must be exactly of 2 characters length.

**Default plan** should be declared for each franchise, it is as SLUG of Plan which is being automatically applied for newly registered franchise users. Default plan should also be included within the assigned plans array.

Expected output - newly created Franchise object:

```
{
  "id": 1,
  "slug": "test-123",
  "name": "test-123",
  "prefix": "t3",
  "logoUrl": null,
  "signUpImageUrl": null,
  "defaultPlan": "free",
  "plans": [1,2,3]
}
```

After franchise is created and we get new franchise id we are able to save added images sending origin and cropped images to endpoints:

uploadUrl: POST `/v1/upload/franchise/${franchiseId}/logo`;

signupUploadUrl: POST `/v1/upload/franchise/${franchiseId}/signup`;

Request JSON body:

```
{
  image: (binary)
  original: (binary)
}
```

Expected output: 
```
{
  imageUrl: "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/franchise/${franchiseId}/signup/d8d13f9eab43efb6dfa8749e08d4e4407af39308.png"
}

and

{
  imageUrl: "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/franchise/${franchiseId}/logo/9d4d79369b548551a2febea0d3e438b3fc01ecbc.png"
}
```


## Franchise Table

Franchise table is a simple table without backend pagination and filtration options.

We refresh Plan table every 20 seconds.


To get a list of all available franchises:
```
GET /v1/franchise
```
Example response without plans:
```
[
    {
        "id": 2,
        "slug": "my-franchise",
        "name": "My Franchise",
        "prefix": "mf",
        "defaultPlan": "free",
        logoUrl: "https://supportlocal-stage.lim.bz/img/support_local.png",
        signUpImageUrl: "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/franchise/1/signup/game-screen1.png"
    }
]
```

```
GET /v1/franchise?plans=1
```

Example response with plans:
```
[
    {
        "id": 2,
        "slug": "my-franchise",
        "name": "My Franchise",
        "prefix": "mf",
        "defaultPlan": "free",
         logoUrl: "https://supportlocal-stage.lim.bz/img/support_local.png",
        signUpImageUrl: "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/franchise/1/signup/game-screen1.png"
        "plans": [
            {
                "id": 1,
                "tag": "free",
                "name": "Plan FREE",
                "stripeId": null
            },
            {
                "id": 2,
                "tag": "standard",
                "name": "Plan STANDARD",
                "stripeId": "membership_monthly"
            },
            {
                "id": 3,
                "tag": "unlimited",
                "name": "Plan FREE and UNLIMITED",
                "stripeId": null
            }
        ]
    }
]
```


Get single franchise details:
```
GET /v1/franchise/{slugId}
```

Example response:
```
{
    "id": 2,
    "slug": "my-franchise",
    "name": "My Franchise",
    "prefix": "mf",
    logoUrl: "https://supportlocal-stage.lim.bz/img/support_local.png",
    signUpImageUrl: "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/franchise/1/signup/game-screen1.png"
    "defaultPlan": "free"
}
```

All endpoints above accepts extra query parameters (note: within URL query path):
- `plans` -> if set to 1 will add to franchise output, plan details assigned to franchise;
- `permissions` -> requires also `plans=1` if set to `1` will add permission list to plans assigned to each franchise.

### Edit Franchise

To update existing franchise:

```
PUT /v1/franchise
```

Request JSON body:
```
{
  "id": 4,
  "slug": "my-franchise-3",
  "name": "My Franchise 3",
  "prefix": "m3",
  "logoUrl": <urlForImage>,
  "signUpImageUrl": <urlForImage>,
  "defaultPlan": "free",
  "plans": [
    {
        "id": 1,
        "tag": "free",
        "slug": "default-free",
        "name": "Plan FREE",
        "stripeId": null,
        "period": null
    },
    {
        "id": 2,
        "tag": "paid",
        "slug": "default-paid",
        "name": "Plan STANDARD",
        "stripeId": "membership_monthly",
        "period": "month"
    }
  ]
}
```

You can update franchise details as above. As a plan list you can forward an array of plan IDs or SLUGs (as on creation example) or forward an array of full Plan objects which must include at least Plan ID parameter.

You can change uploaded images using the same endpoints as for adding new franchise.

**Note: ** - if you don't change the image , the image will not be updated.