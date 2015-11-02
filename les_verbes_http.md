## HTTP methods
HTTP methods (also called verbs) allow us to classify REST services into three categories :
- idempotent verbs
- neutral verbs
- The POST verb

### Idempotence
A verb is said to be idempotent when the application of this verb to the URI produces the same result whether applied once or many times.

![Tip](lightbulb1.png) The PUT and DELETE (except in certain cases for this last one) verbs are idempotent


### Neutrality
A verb is said to be neutral when it does not produce any side effect on the server.
A neutral verb is thus also idempotent

![Tip](lightbulb1.png) The verbs GET, HEAD, OPTIONS and TRACE are neutral verbs.

 ### The GET verb for reading
The GET method is used to get a resource. When it succeed, this method returns a JSON/XML representation of the resources and a HTTP Code 20X.
In case of failure, this method returns in most cases a 404 (NOT FOUND) of 400 (BAD REQUEST) response code.

Examples:
```
GET http://api.europcar.com/api/cars/12345 : return vehicle 12345
GET http://api.europcar.com/api/cars/12345/drivers : returns all drivers of vehicle 12345
```

The  HTTP GET verb should be used exclusively for neutral operations.

 ### The PUT verb for updates

 The PUT verb is used exclusively in the context of an update operation on the server. The body of the request should contain the representation of the updated resources in JSON/XML format
The PUT method may also be used in the context of resource creation, but only if the choice of the resource unique identifier is the responsibility of the client.

When the operation succeed, the response code to return to the client is one of the following:
- 200 code if the resource has been updated and the response body is not empty
- 204 code if the resource has been updated and the response body is empty
- 201 code if the resource has been created. In that case, the response body cannot be empty.

PUT is not a neutral operation, but it is an idempotent one. It is thus important to ensure that multiple calls to the same method produce the same effect.

Examples :
```
PUT http://api.europcar.com/cars/12345 : Update vehicle 12345
PUT http://api.europcar.com/cars/12345/drivers/67890 : Update driver 67890 of vehicle 12345
```

 ### The POST verb for creating resources
 The POST verb is often used to create new resources. It is the server responsibility to assign a unique identifier to the resource.
 
Examples:
```
POST http://api.europcar.com/cars : Create a vehicle, data related to the vehicle are sent in the body of the request.
POST http://api.europcar.com/cars/12345/drivers : Create a driver for vehicle 12345. data related to the driver are embedded in the body of the request.
```
When it succeeds, the 201 HTTP response code is sent back with the header "Location" set to tje link that directly reference the resource that has just been created.


 ### Why and when to use POST instead of PUT ?
 ![Tip](lightbulb1.png)Use PUT when :
 - The resource exist and must be updated
 - The resource does not exist and it is the client responsibility to assign the unique identifier to the new ly created resource.

 Use POST when :
 - The resource does not exist and the unique resource identifier is set by the code on the server
 - The operation is neither idempotent, neither neutral.


 ### The DELETE verb for deletion
The DELETE verb is used to deleted the resource identified by the URI.

Examples:
```
DELETE http://api.europcar.com/cars/12345 : Delete vehicle 12345
DELETE http://api.europcar.com/cars/12345/drivers : Delete all drivers of vehicle 12345
```

According to the HTTP specification, the DELETE method is idempotent because suppressing the same resource multiple times produces the same result, since the resource is no more there after the first call.

### HTTP response codes
The table below lists the HTTP codes it is recommended to return when the operation is applied to a list oa a single resource.

| HTTP Verb | /cars | /cars/{id} |
| -- | -- | -- |
| GET | 200 (OK), returns a list of vehicles.| 200 (OK), returns a single vehicle. 404 (NOT FOUND) if the identifier is not found or is invalid. |
| PUT | 404 (NOT FOUND) | 200 (OK) or 204 (NO CONTENT). 404 (NOT FOUND) if the identifier is not found or is invalid. |
| POST | 201 (CREATED), wih the header "Location" set to the link /cars/{id} where {id} designates the new identifier| 404 (NOT FOUND) |
| DELETE | 404 (NOT FOUND), except if we want to delete the whole collection. | 200 (OK). 404 (NOT FOUND) if the identifier is not found or invalid.   |

The verbs used in the context of Eurpcar are : GET, POST, PUT, DELETE

