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
### Les règles de nommage
# exposition d'objets
Les objets sont exposés au client comme une hiérarchie de composition d'objets. C'est cette composition qui doit permettre d'obtenir l'URI d'accès à la ressource.

Considérons le schéma relationnel suivant :

![Rental Diagram](rent2.png)

- Une location est le rapprochement d'un ou plusieurs drivers à un et un seul véhicule.
- Un Driver peut n'avoir jamais loué de véhicule
- Un véhicule peut ne jamais avoir été loué.

Les structures d'association suivantes sont alors possibles :

* todo Utiliser une logique d'association
* Tous les objets top level doivent avoir leur API
* Ensuite elles peuvent apparaître en sous ressource.


|Type URI | Choix de conception |
| -- | -- |
|URI1 |![](rent3.png)|
|URI2 |![](rent5.png)|

Le choix de l'une ou de l'autre des modélisations est entièrement à l'appréciation de l'analyste métier et de la vue hiérarcchique qu'il souhaite donner au système d'informations. Les URIs suivantes sont équivalentes :

|Verbe HTTP | URI1 | / URI2 |
| -- | -- | -- |
| Description |l'analyste métier impose uniquement la connaissance du contrat de location pour accéder au drivers de la dite location.| Dans le second cas, l'accès au drivers impose de connaître également le véhicule utilisé pour la location. |
| GET | /rents/12345/drivers |/cars/6785/rents/1/drivers |

Dans les deux cas, rien n'empêche l'analyste métier de permettre l'accès aux drivers, véhicules ou locations directement via une clef d'accès unique. Là encore il s'agit d'un choix métier quant à l'exposition des ressources via l'API REST.

Lors de l'accès à un objet, on peut vouloir le discriminer par un attribut. Lorsque ceta ttribut identifie de manière unique dans la hiérarchie accédée la ressource, alors il fera partie de l'URI. Dans les autres cas, ce sera un critère de recherche. 

Dans l'exemple ci-dessous, on souhaite les véficule de type VW.

``` .../cars/VW ``` est à proscrire car plusieurs véhicules peuvent être de type VW. uen requête correcte est la suivante : ``` .../cars?type=VW``` et le retour sera une collection.


## exécution d'opérations
Dans les cas métiers (autres que CRUD), l'invocation d'un service REST renvoie généralement le résultat de l'exécution d'un acte métier. On peut les actions métiers suivantes :
- Signer le contrat
- Valider la location
- Notifier le client


Dans ces cas, on remplacera le verbe par le substantif adéquat comme le montre le tableau ci-dessous :


| Action | Exemple d'URI | Description|
| -- | -- | -- | -- |
| Signer le contrat | POST /rents/12345/signature | Créer une signature et l'associer au contrat |
| Valider la location | PUT /rents/12345/validation | Mettre à jour du statut de validation |
| Notifier le client | POST /drivers/789067/notification | Création d'une notification et association au driver |

Dans le tableau ci-dessus, la description indique la perception qu'a le client du service REST. Dans la réalité, les opérations qui ont lieu sur le S.I. sont beaucoup plus riches et complexes.


Le choix du verbe (PUT, GET , POST, DELETE) dépendra de l'idempotence et de la neutralité de l'action que l'on souhaite exécuter. 



* todo Conserver l'intention métier 
* todo /{id}/views/{viewname}. uniquement si accès à autre chose que la vue par défaut
* 