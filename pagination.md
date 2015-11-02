## Paging

When the volume of data to return is important, it may be helpful to divide the result pages to allow a better reading of the data on the client-side and to limit the use of the bandwidth.

Pagination is to return a number of items from a given position. The 0 position describing the start of the collection.

Both information "position" and "number of items" must be passed by the query to limit the number of items to be returned by the server.

Paging can be initiated by the client or the server when the data volume is too high. In all cases, the server can impose its paging rules (which would induce a gap between customer demand and the actual result).

### Pagination with the header
In this case, the "Range" HTTP header is used with respect to the following format:

```items={position}-{nombre}```.

Ainsi une requête avec l'entête HTTP suivant renverra 20 items à partir du troisième item :
```
Range: items=2-22
```

Cet entête se lit comme suit : Renvoyer 20 items à partir de l'item à la position 2.


 ### dans la query string
Dans ce cas on n'indique pas par un intervalle mais par une position initiale et un nombre maximum d'éléments à renvoyer.
Ainsi la requête suivante renverra les 20 premiers clients à partir de la position 2 incluse.
```
GET http://api.europcar.com/customers?offset=2&limit=20
```

![Tip](lightbulb1.png) Privilégier la pagination au travers de paramètres dans la query string.

### Format de la réponse
 Uen requête de pagination, que ce soit via l'entête ou la query string doit renvoyer le nombre d'items retournés et le nombre total d'items.
 Cet entête se présente comme suit et indique que les 50 premiers items sur un total de 97 ont été renvoyés dans la réponse :
```
Content-Range: items 0-49/97
```

Dans l'exemple suivant, on indique que les 10 derniers items ont été renvoyés
```
Content-Range: items 87-96/97
```

Lorsque le nombre total d'items n'est pas connu au moment de la requête, il est possible de remplacer le nombre total par l'astérisque comme suit :
```
Content-Range: items 0-49/*
```


