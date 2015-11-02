### Representation of data in Input / Output

#### Objets

- When an object is returned, it must be self-described. This means that one will find all the criteria that enabled to return the object in the object returned back if they are part of the returned object. So when accessed by ID, the ID will also be included in the returned object. ``` / Orders/12345 ``` will return a command as follows:
 

``` 
{
 "id":"12345", // id is included in the response despite being in the request URI
  ...
} ```


- the returned object is never named. If it is a JSON structured type, it will appear with the attributes framed by an opening brace and a closing brace, as is the case in the example above. No attribute must prefix the returned object.


- Exposure of a resource must ignore its real representation in the information system (see previous example about pattern). If there is a resource resulting from the aggregation of a number of other resources, then it must be seen as an compound object .

![Tip](lightbulb1.png) When there is a composition relation (UML sense), the whole object(or a view of it) must be returned.

When there is a association relation( UML sense), You should return the minimum namely id and functional name for example.


#### Views

![Tip](lightbulb1.png) When it is desired to access a particular view of a resource, for example the client identity or retrieve only its payment data. You need to  prefix the view that you want to access by the word ```views```:

```
/customers/12345/views/ident
/customers/12345/views/payment
```

The absence of the prefix ```views``` means that we want the default view: ``` /customers/12345 ```.



#### Collections
The objects are sent as a homogeneous list of data, framed by an opening bracket and closing bracket. As before, the lists are objects and therefore should never be named. Below is an example of a list with two items:
``` 
[
    {
        "id":"12345",
        "firstName": "ABC",
        "lastName":"DEF"
        ...
    },
    {
        "id":"57643",
        "firstName": "XYZ",
        "lastName":"UVW"
        ...
    }
]
```

A URI that returns a list of preferences ended with a segment in the plural as in the following example:
```
GET http://api.europcar.com/orders/1234/linetitems
```

This call returns the default view. To send a specific view, we can suffix by  ```views/nameoftheview```. In the example below we want to return the id and name of each order line.


```
GET http://api.europcar.com/orders/1234/linetitems/views/names
```
will produce the following response :
``` 
[
    {
    "id":"12345",
    "name": "ABCF DEF"
    },
    {"id":"57643",
    "name": "XYZ UVW"
    }
]
```


#### Dates
Dates are transmitted in two different ways depending on whether they are specified in an HTTP header or in the body of the request / response.

In the HTTP header, dates must comply with RFC 1123 by transmitting the date in the following format:
````
Mon, 3 Aug 2015 09:26:12 GMT
````
That date as shown does not have access to milliseconds. The Java pattern to apply to get it is
```
EEE, dd MMM yyyy HH:mm:ss 'GMT'
```

In the body of the request or response, the dates are sent to ISO8601 Format:
```
yyyy-MM-dd'T'HH:mm:ss.SSS'Z'
```

Pour une date sans heur, il faut quand même transmettre l'information horaire qui pourra être ignorée par l'applicatif.




