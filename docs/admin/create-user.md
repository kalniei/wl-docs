# Create User

Admin View && Distributor only.
Here admin can create new users and assign them roles.

Endpoint for creation user:

```
POST /v1/users
```

Request JSON body:

```
{
  email: string
  password: string,
  enabledFbPageAccess: boolean,
  enabledInstagramAccess: boolean,
  roles: [ ROLE_USER | ROLE_DISTRIBUTOR | ROLE_SUPER_ADMIN ],
  franchise?: string,
}
```

Expected output - newly created User object

`franchise` property is optional. It expects slug of the chosen franchise. If chosen - user will be created under that franchise.

We get available list of franchises via:

```
GET /admin/franchise
```

Expected output - list of all franchises object, available for user.
