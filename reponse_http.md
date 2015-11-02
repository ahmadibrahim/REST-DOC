## La réponse du service REST
The response structure of a REST service call must be:
- An HTTP response code 
- A status 
- A message 
- The data.

### The HTTP response code
This is an integer indicating the HTTP response code. This code may also be included in the body of the response in the form of an attribute

### l'attribut status
This attribute contains one of the 3 following: 
- "fail" for HTTP response codes 5XX
- "error" for HTTP response codes 4XX 
- "success" for HTTP response codes 1XX, 2XX and 3XX.

### The message attribute
This attribute is only present when the status is "done" or "error". It contains a list of error messages to be considered by the customer.

A message is an object with two attributes ```code``` and ```description```.

In some cases at the margin, it may be useful to have more detailed messages, in which case it will be necessary to include a ```details``` attribute that contains a list of detailed error messages.


### The data attribute
Contains the body of the response. On error (status equal to "fail" or "error"), the data contains the list of exceptions lifts.

Example 1:
```
{
"code":200,
"status":"success",
"data": [{"name":"Vincent"}]
}```


Example 2:
```
{
"code":400,
"status":"error",
"message" : 
    {
        "code": UserAbsent",
        "description" : "The request is invalid. Rent no more present"
    }
"details" : 
    [
        { 
            "code":"InvalidArgument", 
            "description" :"Parameter ABC should be a valid date"
        }, 
        { 
            "code":"UnknownField", 
            "description" :"Unknown Field XYZ"
        }
    ]
}```

