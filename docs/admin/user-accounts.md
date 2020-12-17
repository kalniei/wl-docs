# User Accounts

Admin View only.
Here admin's can manage your accounts.

## Find Account

The Search filters, that are filtering the Accounts Table

1. Email Address
2. First Name
3. Last Name
4. Company Name
5. Company Keyword
6. Status - dropdown, allowed values: **All**, **Active**, **Inactibe**

Button Search will trigger request to Api: 

```
v1/admin/users
```
with optional additional filters params: `filters[]=`

```
email: string, - operand "l"
firstName: string,  - operand "l"
lastName: string,  - operand "l"
companyName: string,  - operand "l"
companyKeyword: string,  - operand "l"
status: string, - operand "eq"
```
## Accounts Table

lorem

### Impersonate User

lorem

### Edit Account

lorem

### QR Codes

lorem

### Delete Account

lorem
