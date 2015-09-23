

|Type URI | Choix de conception |
| -- | -- |
|URI1 |![](rent7.png)|
|URI2 |![](rent10.png)|

Le choix de l'une ou de l'autre des modélisations est entièrement à l'appréciation de l'analyste métier et de la vue hiérarchique qu'il souhaite donner au système d'informations. Les URIs suivantes sont équivalentes :

|Verbe HTTP | URI1 | / URI2 |
| -- | -- | -- |
| Description |l'analyste métier impose uniquement la connaissance de la commande pour accéder aux lignes de commande de la dite commande.| Dans le second cas, l'accès aux lignes de commande impose de connaître également le client pour de la commande. |
| GET | /rents/12345/drivers |/cars/6785/rents/1/drivers |

Dans les deux cas, rien n'empêche l'analyste métier de permettre l'accès aux commandes, clients ou locations directement via une clef d'accès unique. Là encore il s'agit d'un choix métier quant à l'exposition des ressources via l'API REST.

Lors de l'accès à un objet, on peut vouloir le discriminer par un attribut. Lorsque ceta ttribut identifie de manière unique dans la hiérarchie accédée la ressource, alors il fera partie de l'URI. Dans les autres cas, ce sera un critère de recherche. 

Dans l'exemple ci-dessous, on souhaite les véficule de type VW.

``` .../cars/VW ``` est à proscrire car plusieurs véhicules peuvent être de type VW. uen requête correcte est la suivante : ``` .../cars?type=VW``` et le retour sera une collection.

TODO Ne pas permettre l'usage des index dans les URI.
## exécution d'opérations
Dans les cas métiers (autres que CRUD), l'invocation d'un service REST renvoie généralement le résultat de l'exécution d'un acte métier. On peut les actions métiers suivantes :
- Signer le contrat
- Valider la location
- Notifier le client


Dans ces cas, on remplacera le verbe par le substantif adéquat comme le montre le tableau ci-dessous :


| Action | Exemple d'URI | Description|
| -- | -- | -- | -- |
| Signer le contrat | POST /rents/12345/signature | Créer une signature et l'associer au contrat |
| Valider la location | PUT /rents/12345/validation | Mettre à valide  du statut de location |
| Notifier le client | POST /drivers/789067/notification | Création d'une notification et association au driver |

Dans le tableau ci-dessus, la description indique la perception qu'a le client du service REST. Dans la réalité, les opérations qui ont lieu sur le S.I. sont beaucoup plus riches et complexes.


Le choix du verbe (PUT, GET , POST, DELETE) dépendra de l'idempotence et de la neutralité de l'action que l'on souhaite exécuter. 

Lorsque l'on souhaite accéder à une vue particulière d'une ressource, par exemple pour le driver récupérer uniquement son identité ou uniquement ses informations de conduite. On préfixera la vue que l'on souhaite accéder par ```views```:

```
/drivers/12345/views/ident

/drivers/12345/views/driveinfo
```

L'absence du préfixe ```views```signifie que l'on souahite la vue par défaut:
```/drivers/12345```.




