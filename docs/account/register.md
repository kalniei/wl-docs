###### Register

Franchise User registration should be allowed on path which would allow to identify franchise the user is signing in. For franchise identification it is recommended to use Franchise slug somewhere within registration URL, for example:
`/my-franchise-slug/register`.

Franchise slug will be the Franchise ID required to be send to the backend on User registration:
POST /v1/auth/register

Example request JSON body:
{
    "franchise": "my-franchise-slug",
    "email": "johny@lim.bz",
    "password": "mypassword123"
} 