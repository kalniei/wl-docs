###### User Accounts

Admin View only.
Here admin's can manage accounts.

## Find Account

The Search filters, that are filtering the Accounts Table

1. Email Address
2. First Name
3. Last Name
4. Company Name
5. Company Keyword
6. Status - dropdown, allowed values: **All**, **Active**, **Inactive**

Button Search will trigger request to Api: 

```
v1/admin/users
```
with optional filters params: `filters[]=`

```
email: string; - operand "l"
firstName: string;  - operand "l"
lastName: string;  - operand "l"
companyName: string;  - operand "l"
companyKeyword: string;  - operand "l"
status: string; - operand "eq"
```

Example of created link: 

```
v1/admin/users?filters[]=email,l,test@lim.bz,&filters[]=firstName,l,testFirst,&filters[]=lastName,l,testLast,&filters[]=companyName,l,testInc,&filters[]=companyKeyword,l,banana,&filters[]=status,eq,active
```
---

## Accounts Table


Paginated list of all users, created under current Admin user.

By default pagination is set to **10** records.
All columns are sortable.
No columns has filters inside the table.

We use this route for getting accounts:

```
GET v1/admin/users
```

Optional parameter for pagination:

```
  page: number;
  itemsPerPage: number; - 5 | 10 | 15
  showAllRecords: boolean;
  sortBy: string;
  sortDesc: boolean;
```

Example of created link: 

```
v1/admin/users?page=1&itemsPerPage=10&showAllRecords=0&sortBy=lastName&sortDesc=1
```

Expected output:

```
{
  data: [
    {
      companyKeyword: "test"
      companyName: "Test Inc"
      email: "test@lim.bz"
      fbBusinessPage: 1111
      instagramId: 1111
      firstName: "Test"
      id: 1
      lastName: "Test"
      status: "active"
    }
    ...
  ],
  state: {
    fields: []
    filterBy: null
    filterValue: null
    filters: []
    itemsCount: 10
    itemsPerPage: 10
    joins: []
    page: 1
    showAllRecords: false
    sortBy: "lastName"
    sortDesc: true
    totalItems: 463
    totalPages: 47
  }
}
```

### Impersonate User

Option to log in into users account.

```
GET v1/users/${id}/?permissions=1
```

After successful response we do all actions like login user.

see: [user login](../account/login.md) for more details

Note - in store we keep `parentUserId" ${adminId}` and  in Local Storage - `impersonate: ${adminEmail}`.

It allowes us to go back to admin.

### Edit Account

lorem

### QR Codes

lorem

### Delete Account

lorem
