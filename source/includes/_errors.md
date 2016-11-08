# Errors

The Life.io User Management API primarily uses the following error codes:


Error Code | Meaning
---------- | -------
401 | Unauthorized -- Your API Key or User Management Admin Token is incorrect.
404 | Not Found -- You've provided a user email, company id, or parent organization name that does not match our records.
422 | Invalid -- You didn't include any of the required parameters!
