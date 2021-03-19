# Reset Password

At the entering Reset Password Page we are checking if there is query param `f`. If there is not - we use defaul winlocal plan and view.

If we do have the query param, we send request to get franchise useing query param:

```
GET /v1/admin/franchise/${queryF}
```

Expected output:

```
{
    defaultPlan: "supportlocal-free"
    id: 1
    logoUrl: "https://supportlocal-stage.lim.bz/img/support_local.png"
    name: "SupportLocal"
    prefix: "sl"
    signUpImageUrl: "https://winlocal-stage-client-panel.s3.us-east-2.amazonaws.com/franchise/1/signup/game-screen1.png"
    slug: "supportlocal"
}
```


In this view we have one fiels to fill: `email`.

On click "Reset" we send

```
POST /v1/auth/reset-password
```

Json body: 

```
{
  email: "test@gmail.com"
}
```

Expected output:

400, when there is no such user:

```
{
  user: ["User not found."]
}
```

**OR**

200, when email with new password was sent

```
["OK"]
```