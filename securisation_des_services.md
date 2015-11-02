## Authenticated access to the services
An authentication is required either at the user level or at the level of the calling party (the partner).

In the first case, the credentials are usually based on a login / password. In the second, it is a token that identifies the third party. In both cases, it is recommended to use the  [Basic HTTP Auth] (http://tools.ietf.org/html/rfc2617) header.

```Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==```

In the context of a token, the same HTTP header is used. However in this case the user and password correspond to a dedicated shared API Key.