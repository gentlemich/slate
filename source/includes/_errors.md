# Errors

The Upcall API uses the following error codes:


```json
{
  "errors": {
    "from_number": [
      "Couldn't find that number"
    ],
    "name": [
      "Campaign name is a required field"
    ]
  }
}
```


Error Code | Meaning
---------- | -------
400 | Bad Request -- The request contained error and could not be completed.
401 | Unauthorized -- The `auth_token` field is missing or incorrect
403 | Forbidden -- The action you attempted is not allowed based on your `auth_token`.
404 | Not Found -- The specified item could not be found

Additionally, error messages will be delivered using structured JSON.
