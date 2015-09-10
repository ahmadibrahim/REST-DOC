## Nommage des ressources (URI)
En plus d'utiliser les verbes HTTP de manière appropriée, il est indispensable de créer des URI qui rendent l'usage de l'API simple et intuitive.

Une API REST est constituée :
- d'une collection d'URIs
- d'appels HTTP à ces URIs (les verbes)
- de représentations XML/JSON des ressources

Chaque ressource du serveur est accessible au travers de l'URI. Par opposition à un verbe qui décrit une action, une ressource est un nom commun qui décrit une entité.
Le nommage des URIs doit suivre une structure hierarchique cohérente afin de garantir son utilisabilité.

### Les bonnes pratiques
Pour illustrer les bonnes pratiques, nous nous appuyons sur les entités clients, commandes, lignes de commandes et produits.

Le nommage des ressources peut être le suivant :

| Ressource | Nommage de la ressource |
| -- | -- |
| Client | customers |
| Commande | orders |
| LigneDeCommande | lineitems |
| Produit | products |

Les ressources sont nommées au pluriel et en minuscules. Ceci est du au fait que le service renverra une collection d'objets lorsqu'aucun identifiant n'est présent dans l'URI.
Ci-dessous quelques exemples d'utilisation:


| Opération | URI|
| -- | -- |
| Création d'un nouveau client | POST http://api.europcar.com/customers |
| Récupération/Mise à jour/Suppression du client 12345 | GET/PUT/DELETE http://api.europcar.com/customers/12345 |
| Création d'une nouvelle commande pour le client 12345 | POST http://api.europcar.com/customers/12345/orders |
| Récupération des commandes du client 12345 | GET http://api.europcar.com/customers/12345/orders |
| Création d'une ligne de commande dans la commande 67890 pour le client 12345 | POST http://api.europcar.com/customers/12345/orders/67890/lineitems |
| Récupération de la première ligne de commande dans la commande 67890 pour le client 12345 | GET http://api.europcar.com/customers/12345/orders/67890/lineitems/1 |

Les exemples ci-dessous illustrent le concept de hiérarchie à appliquer et la traversabilité de la hiérarchie au travers d'un identifiant unique qui vient s'intercaler entre les noms communs représentants les entités traversées.

 ### Pratiques à éviter absolument
Le premier anti-pattern consiste à regrouper les services REST au sein d'une même URI et d'utiliser les paramètres de la query-string pour distinguer les services les uns des autres.
Par exemple, l'URI de mise à jour d'un client pourrait être :
```
GET http://bad-api.europcar.com/services?op=update_customer&id=12345&format=json
````

Le problème de cette URI est qu'elle n'est pas auto-décrite.
1. le verbe neutre GET désigne une opération qui ne l'est pas.
2. L'URI possède une structure hiérarchique identique pour l'ensemble des ressources ne permettant pas de différencier les ressources accédées juste au travers de la lecture de l'URI.

Un autre contre-exemple est indiquer l'opération dans l'URI
```
GET http://bad-api.europcar.com/customers/12345/create
```

L'URI doit désigner une resssource et non une opération sur une ressource. L'URI doit être identique quelque soit l'opération qui sera effectuée sur la ressource (POST/ GET / PUT / DELETE)

### Pluriel versus singulier
La règle à respecter est la suivante. Lorsque la ressource désigne une collection d'objets alors il faut utiliser le pluriel.

Mais que se passe-t-il lorsqu'aucune ressource n'existe réellement sur le serveur ?
Considérons que nous souhaitions construire un service qui vérifie que l'email saisi respecte un pattern bien défini.

Il s'agit d'une opération neutre, le verbe HTTP approprié est le verbe GET.


Dans notre cas, il n'y a pas d'entité Pattern en base, nosu serions tenté d'utiliser le singulier avec une URI du type :
```
GET http://bad-api.europcar.com/pattern-valid
```

Cela viole le principe hiérarchique. Une solution est de considérer qu'il existe  une multitude de patterns, nous pouvons donc en déduire que le pattern est une entité virtuelle qui sera désignée par un nom au pluriel. Pour valider l'email, nous pourrons considérer que l'identifiant "email" désigne notre patterns dans cette collection virtuelle inifinie de patterns.


L'URI de désignation du pattern pourra alors être :
```
GET http://api.europcar.com/patterns/email
```

La validité pourra être considérée comme une propriété et donc sera plus bas au niveau hiérarchique. L'URI définitive devient alors :
```
GET http://api.europcar.com/patterns/email/validity
```

Cette URI désignant de manière unique la ressource, la valeur saisie par le client sera transmise dans la query string. Cela nous donne l'appel suivant :

```
GET http://api.europcar.com/patterns/email/validity?value=name@domain.com
```

### Représentation des données en entrée / sortie
#### objets
#### collections
#### les intervalles
#### les règles de nommage
#### Les dates
Les dates sont transmises de deux manières différentes selon qu'elles soient précisées dans un entête HTTP ou dans le corps de la requête / réponse.

Dans l'entête HTTP, les dates doivent respecter la RFC 1123 qui consiste à transmettre la date au format suivant :
````
Mon, 3 Aug 2015 09:26:12 GMT
````
Cette date comme on peut le voir ne permet pas d'avoir accès aux millisecondes. La pattern Java à appliquer pour l'obtenir est
```
EEE, dd MMM yyyy HH:mm:ss 'GMT'
```

Dans le corps de la requête ou de la réponse, les dates sont envoyées au format ISO8601 :
```
yyyy-MM-dd'T'HH:mm:ss.SSS'Z'
```

