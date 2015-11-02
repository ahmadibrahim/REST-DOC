## Versioning of services
### By using content negotiation

This approach is suitable when the version of the service has not changed, but that there are several possible representations of a resource. In this case, the HTTP header "Accept" is used. In the following example, we request a resource in JSON format in version 1.

```
Accept: application/json; version=1
```

The response contains in the "Content-Type" HTTP header the format and the version of the resource. In the example below, the header indicates that th response contains the resource in JSON format in version 1
```
Content-Type: application/json; version=1
```

If the client does not request a specific version, the server should return the oldest version supported by the resource. This rule also applies when creating or updating a resource. The data transferred should be considered to be in the oldest supported representation.

When the requested version or format do not exist or are no longer supported, the HTTP response code 406 (NOT ACCEPTABLE) must be returned.

### Using the URI
In this case the URI is prefixed by a version number vXXX where XXX denotes the version number. In the example below we address service version 2:

```
GET http://api.europcar.com/v2/customers/12345
```

This will allow to place URL routing rules quite easily at the intermediate HTTP servers.


![Tip](lightbulb1.png) Versioning with the URI is preferred. 

### Conditions for the creation of a new API version ?

As soon as backward compatibility is broken, it is imperative to create a new version. This concerns in particular the following cases:

- Renaming an attribute or entity 
- Deleting an attribute or entity 
- Changing the type of an attribute 
- Modification of validation rules of an attribute or entity that breaks compatibility with the existing version. 

By cons, in the cases below, it is not necessary to create a new version of the API: 

- Adding new attributes to the answer 
- Support for new formats 
- Support for new languages

### Depecating a service
If a service is deprecated but continues to be supported, this should be indicated in the "Deprecated" header which is of type boolean:
```
Deprecated: true
```
