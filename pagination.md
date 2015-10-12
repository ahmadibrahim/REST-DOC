## Pagination
Lorsque le volume de données à renvoyer est important, il peut être utile de découper le résultat en pages afin de permettre une meilleure lecture des données côté client et de limiter l'usage de la bande passante.
La pagination consiste à renvoyer un certain nombre d'éléments à partir d'une position donnée. La position 0 décrivant le début de la collection.
Ces deux informations, "position" et "nombre d'items" doivent être transmis par la requête pour limiter le nombre d'éléments à renvoyer par le serveur.
La pagination peut être à l'initiative du client ou du serveur lorsque le volume de données retourné est trop important. Dans tous les cas, le serveur peut imposer ses règles de pagination (qui induirait un décalage entre la demande du client et le résultat effectif).

### Pagination avec le header
Dans ce cas, on utilise l'entête HTTP "Range" qui respecte le format suivant : items={position}-{nombre}.
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


