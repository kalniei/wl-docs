# Register

We can sign up uder default user, or under franchise user.

At the entering Register Page we are checking if there is query param `f`. If there is not - we use defaul winlocal plan and view.

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

In this view we have two fields to fill: `email` and `password`.

On Sign Up click we send:

```
POST /v1/auth/register
```


Example request JSON body:

```
{
    "franchise": "my-franchise-slug",
    "email": "johny@lim.bz",
    "password": "mypassword123"
}
```


Expected output: 

```
{
    emailConfirmation: true
}
```

It will get us to [Email Confirmation Page](confirmation-link.md)

On success registration user will recieve email with conformation link.