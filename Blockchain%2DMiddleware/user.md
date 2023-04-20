# get
Route: pgsql/user/get/username
Authorization: bearer [env]
Body:
```
{
    "data": {
        "username": "aws-username"
    }
}
```

---

# add
## one
Route: pgsql/user/add/one
Authorization: [user_auth]
Body: none

---

# delete
## one
Route: pgsql/user/delete/one
Authorization: bearer [env]
Body:
```
{
    "data": {
        "userId": 1
    }
}
```