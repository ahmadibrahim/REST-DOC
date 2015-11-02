##Session management
ST in REST means State Transfer. This means that session information must be transferred to the client and not stored as session objects on the server side. The rule is the following:
- Any state specific to the current session must be stored on the client side
- Any state specific to a business object and session independent must be stored on the server side.

The session may be useful in 

The session may be useful in some cases like :
- Compatibility with previous versions of the application
- Preloading of heavy resources to improve performance

In this case, a session ID is transmitted to the client through a cookie in the HTTP header.


```
Set-Cookie: jsessionid=ig2fac55; path=/; secure; HttpOnly
```


- The ```secure``` flag prevents the cookie from being sent in clear text over HTTP channel and forces the use HTTPS.
- The  ```HttpOnly``` flag prevents any JavaScript code to access the cookie, this practice is common in XSS attacks (Cross site scripting). 

These flags are to be set by the application.
