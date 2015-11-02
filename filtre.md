## Filtrage

Filtering apply to search operations on resources.

The filtering options are placed in the query-string only as an additional parameter: "filter". The criterion name is separated from its value by the string "::" and the filter criteria are separated from each other by the '|' character. 

The API specification must specify the attributes on which research can be conducted. The search criteria apply to the resource indicated by the URI. The naming of the parameters  is independent of the naming attributes of the considered resources. 

Thus, the following URI could help capture customers with an order containing a single article and with an amount  higher than €1,000:

```
GET http://api.europcar.com/orders?filter=amount:gt:1000|count::1
```

One can complement the filter criteria by time intervals, adding parameters "before" and "after". So in the following example we get the previous commands between 1st of January 2015 and 15th of January 2015.

```
GET http://api.europcar.com/orders?filter=amount:gt:1000|count::1&after=timestamp&before=timestamp
```

When richer operators are needed, it may be necessary to define a dedicated syntax. For the inequality we can then use the following syntax:

| Opérator | Description |
| -- | -- |
| :: | Equality |
| :lt: | Less than |
| :gt: | Greater than |
| :le | Less than or equal |
| :ge: | Greater than or equal |
| :starts-with: | Starts with |
| :contains: | Contains |
| :end-with: | ends with |

Pour les opérations de filtre encore plus complexes, il convient alors d'utiliser la librairie Apache Olingo qui implémente le concept de query défini dans [l'OpenData protocol (OData) Filter System Query Option ](http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398094)


###Filtres prédéfinis
Dans des cas complexes, certains filtres peuvent être prédéfinis sur le serveur et nommés. Dans ce cas, l'URI aura la forme suivante :

````
GET http://api.europcar.com/orders?filtername=name&param1=val1&...
````

Dans cet exemple, le filtre est nommé ```name``` et les paramètres attendus par le filtre sont passés dans l'URI.


Lorsque l'on souhaite restituer une vue particulière, on utilisera le mécanisme de vue décrit précédemment.

Dans l'exemple ci-dessous, la liste des commandes renvoyées va restreindre le résultat au attributs définis par la vue ```amounts```.
```
GET http://api.europcar.com/orders/views/amounts?filtername=name&param1=val1&...
```

