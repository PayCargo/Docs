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


Transaction Make Payment Response codes:

Error Code | Meaning
---------- | -------
411 | Transaction is not status created or proofed, cannot be approved
412 | Overnight availability reached, insufficient funds, cannot be approved
414 | Transaction does not exist, cannot be approved
431 | Vendor is not active, cannot be approved
432 | Payer is not client registered, cannot be approved
433 | Vendor is not client registered, cannot be approved
441 | Missing Vendor Credit Account, cannot be approved
442 | Missing Payer Debit Account, cannot be approve
200 | Transaction paid