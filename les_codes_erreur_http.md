## HTTP error codes
The exhaustive list of HTTP error codes is available at the following address : [restapitutorial](http://www.restapitutorial.com/httpstatuscodes.html)

The main HTTP  REST codes are : 

| Code | Label | Description |
| -- | -- | -- |
| 200 | OK | Generic success code |
| 201 | CREATED | Resource successfully created via POST or PUT and the Location header set to the new created resource. |
| 204 | NO CONTENT | when the body of the response is empty for a successfull request. |
| 304 | NOT MODIFIED | In response to a conditional GET request that indicates that the resource has not been modified since the date set in the header of the request |
| 400 | BAD REQUEST | Request parameters are invalid |
| 401 | UNAUTHORIZED | No authentication token found in the request |
| 403 | FORBIDDEN | User not authorized to access the resource |
| 404 | NOT FOUND | The requested resource has not been found |
| 409 | CONFLICT | When the request is likely to create conflicts such as duplicates or unsupported cascading deletes  |
| 500 | INTERNAL SERVER ERROR | Generic server side error |
