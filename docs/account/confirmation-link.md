# Confirmation Link

On success registration user will recieve email with confirmation link.

We also get transfered to Conformation Email page, where you can reenter your email in case the link was not send.

On Entering email and clicking **"Send Link Again"** we send:

```
POST /v1/auth/email-confirmation/resend
```

Json body: 

```
{
  email: test@gmail.com
}
```

Expected output:

200, if link is sent:

```
{
  resent: true
}
```

400, if such email was never registered:

```
{
  email: "Not found."
}
```

400, when email address has been already confirmed.

```
{
  email: "Email address already confirmed."
}
```



Example of confirmation link: https://client.winlocal.xyz/?u=NmVjMmQ2NDEtZDA1Zi00YTU0LWI5MzItYjIxYjU3MzM2NmFi

When we enter this link the following has been sending:

```
GET /v1/auth/email-confirmation/${uFromQueryParam}
```

Expected output:

400, when link is expired

```
{
  token: ["Confirmation not found or has expired."]

}
```

200, when email confirmed

```
{
  confirmed: true
}
```