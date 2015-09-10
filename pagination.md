## Pagination
Lorsque le volume de données à renvoyer est important, il peut être utile de découper le résultat en pages afin de permettre une meilleure lecture des données côté client et de limiter l'usage de la bande passante.
La pagination consiste à renvoyer un certain nombre d'éléments à partir d'une position donnée. La position 0 décrivant le début de la collection.
Ces deux informations, "position" et "nombre d'items" doivent être transmis par la requête pour limiter le nombre d'éléments à renvoyer par le serveur.

### Pagination avec le header
Dans ce cas, on utilise l'entête HTTP "Range" qui respecte le format suivant : items={position}-{nombre}.
Ainsi une requête avec l'entête HTTP suivant renverra 20 items à partir du troisième item :
```
Range: items=2-22
```

Cet entête se lit comm suit : Renvoyer 20 items à partir de l'item à la position 2.


 ### dans la query string
Dans ce cas on n'indique pas par un intervalle mais par une position initiale et un nombre maximum d'éléments à renvoyer.
Ainsi la requête suivante renverra les 20 premiers clients à partir de la position 2 incluse.
```
GET http://api.europcar.com/customers?offset=2&limit=20
```

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


## Filtrage
Les options de filtrage sont placés dans la query-string uniquement sous la forme d'un paramètre supplémentaire : "filter".
Le nom du critère est séparé de sa valeur par le chaine de caractères "::" et les critères de filtre sont séparés les uns des autres par le caractère '|'.

Ainsi, l'URI suivante pourrait permettre de récupérer les véhicules de la marque Volskwagen qui sont garés à Paris:

```
GET http://api.europcar.com/cars?filter=city::Paris|constructor::VW
```

On peut complémenter les critères de filtres par des intervalles temporels  en ajoutant les paramètres "before" et "after". Ainsi dans l'exemple suivant on obtient les véhicules de la marque VW garées à Paris entre le 1er janvier 2015 et le 15 janvier 2015.

```
GET http://api.europcar.com/cars?filter=city::Paris|constructor::VW&after=timestamp&before=timestamp
```

Dans les exemples de filtre ci-dessus, seule une égalité parfaite est utilisée. Lorsque l'on souhaite des opérateurs plus riches, il faut alors définir une syntaxe dédiée. Pour l'inégalité on pourra alors utiliser la syntaxe suivante :

| Opérateur | Description |
| -- | -- |
| :: | Egalité |
| :lt: | Inférieur |
| :gt: | Supérieur |
| :le | Inférieur ou égal |
| :ge: | Supérieur ou égal |
| :starts-with: | Commence par |
| :contains: | Contient |
| :end-with: | Se termine par |

Pour les opérations de filtre encore plus complexes, il convient alors d'utiliser la librairie Apache Olingo qui implémente le concept de query défini dans [l'OpenData protocol (OData) Filter System Query Option ](http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part2-url-conventions/odata-v4.0-errata02-os-part2-url-conventions-complete.html#_Toc406398094)

