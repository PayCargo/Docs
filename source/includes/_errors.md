# Errors

The Paycargo API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API token is wrong.
403 | Forbidden -- The user associates with the token does not have sufficient permissions
404 | Not Found -- The specified resource could not be found.
422 | Unprocessable Entry -- Please change your request parameters
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
