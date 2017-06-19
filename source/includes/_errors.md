# Errors

The Payment Processor API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request had
403 | Forbidden -- you did not supply a valid Project Key and cannot access this route.
404 | Not Found -- You requested a route or format that does not exist.
405 | Method Not Allowed -- You tried to access a route with a method that is not allow.
415 | Unsupported Media Type -- you made a POST request with a content-type header that is not supported.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
