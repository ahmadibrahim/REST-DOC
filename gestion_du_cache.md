## Cache management

The cache is exclusivement used for GET resquests. The following headersare used to support the cache mechanism:

| Header | Description | Example |
| -- | -- | -- |
| Date | Date and time at which the resource was sent (RFC 1123 format) | Mon, 3 Aug 2015 09:26:12 GMT |
| Cache-Control | How long (in seconds) the response should stay in the cache. The value no-cache means that the response should not be cached at all. | Cache-Control: 3600 ou Cache-Control: no-cache |
| Expires | Resource Expiration date (format RFC 1123).  | Mon, 3 Aug 2015 09:26:12 GMT |
| Pragma | If the resource should not be cached, then this header is set to the value no-cache | Pragma: no-cache |
| Last-Modified | Date of last resource update (RFC 1123 format)  | Mon, 3 Aug 2015 09:26:12 GMT |


![Tip](lightbulb1.png) Cache rules are often expressed at the infrastructure level. They are not generally set in the application.