# Create User

Admin View only.
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
  roles: [ ROLE_USER | ROLE_RESELLER | ROLE_SUPER_ADMIN | ROLE_MULTI_USER ],
}
```

Expected output - newly created User object


**NOTE** At this point no matter what role we create - we only can get role ROLE_USER.
Other roles should either be removed or backend should be able to create other roles accounts