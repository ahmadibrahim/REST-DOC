## The six constraints of REST
### Uniform Interface

This constraint is defined by the fact that each resource is uniquely identified through a URI. The server returns a representation of the resource to the client with respect to a predefined schema that is independent of the physical representation of the resource on the server. The most common formats are JSON and XML encoded as UTF-8 entities.

### Stateless
Stateless approach is key to the REST architecture style. It implies that all the data required to service the request is contained within the URI, the URI parameters (query-string), the body of the request and/or the headers. Once the request is serviced, the server returns the exhaustive information required to submit future requests through the headers, the body and the response status code.



This is a breaking change with the classical approach where data is stored in a server side session store and made available to any future requests from the same user in the same session. REST requires that each client includes all the data required by the server to execute the request.

The stateless approach brings more scalability to applications since the server does not need to maintain and/or replicate the session state. Moreover, load balancing components do not have to be aware of the session affinity that are usually complex to implement at the infrastructure level and makes deployment of a new version of the application less smooth since it requires all sessions to be shutdown first.


P.S. Nonetheless, it is not always possible to have a stateless approach . Services like OAuth require information to be maintained in the session store. Compatibility with previous versions of the application may also be a motivation to maintain session data across requests

When maintaining a session is essential, you must :
- Make sure that the data in the session store are not used as a temporary state between HTTP requests but exclusively used as a cache holding data that will stay valid during the whole session.
- Make sure that the session is available across the whole server farm (no session affinity).


### Cache enabled
Proxies as well as Web clients (whether desktop of mobile) are able to store responses in their own cache. Responses should thus, implicitly or explicitly, define themselves as cache candidates or not to avoid useless requests or use of obsolete data. Good use of caching may greatly improve scalability and performance.

### Client / Server
It is the client that initiates the request.
The client and the server exchange data whose representation is independent from both, the server side data representation and the client side user interface. This allows both parties (client and server) to evolve independently from each other as long as the data interchange format is respected and stays compatible.

### Layered System
A client cannot guess if he is directly connected to the target application server or if he is served by a proxy server. Usually theses proxies may be a load balancer, a shared cache service or an authentication and habilitation service.


###Code on demand (Optional)
Of the six constraints required by REST, this constraint is the single optional one. It allows the client to return code that will be executed on the client side to extend server functionality. This code may be a Java applet or a JavaScript script to access local peripherals.

Note : Europcar has not planned to use "code on demand".




## Concepts
- In REST, the key abstraction to designate the unit of information is the resource. Any information that can be named is potentially a resource. More generally, any concept that may be designated by a hypertext reference must be considered as a resource.
- A resource is accessed through a URI.
- A URI may be invoked through on or multiple methods (GET / PUT / POST / DELETE) depending on the action we went to execute (Read / Update/ Create / Delete).
- A REST Service invocation may include in the body of the request input parameters that represent the business object and get back in the response a representation of the business object.
- Many variations of a representation are possible. The most widely used are JSON and XML.

The diagram below illsutrates those concepts.

![Concepts REST](rest-concepts.png)

Figure : http://www.apigee.com

