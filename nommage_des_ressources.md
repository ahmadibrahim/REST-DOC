## Naming resources (URI)
In addition to using HTTP verbs properly, it is essential to create URI that make the use of simple and intuitive API.

A REST API consists of: 
- A collection of URIs
- HTTP calls to these URIs (verbs) 
- XML representations / JSON resources

Each resource on the server server is accessible through the URI. As opposed to a verb that describes an action, a resource is a common name that describes an entity. The URI naming must follow a coherent hierarchical structure to ensure its usability.


### Best Practices
To illustrate the best practices, we rely on customers entities, orders, order lines and products.

The naming of resources may be the following:

| Ressource | Nommage de la ressource |
| -- | -- |
| Client | customers |
| Order | orders |
| Order Line | lineitems |
| Product | products |

![Tip](lightbulb1.png)Resources are named in the plural and in lower case.

This is due to the fact that the service will send a collection of objects when no identifier is present in the URI. 
Below are some examples:


| Operation | URI|
| -- | -- |
| Create a new customer | POST http://api.europcar.com/customers |
| Get/Update/Delete customer 12345 | GET/PUT/DELETE http://api.europcar.com/customers/12345 |
| Create a new order for customer 12345 | POST http://api.europcar.com/customers/12345/orders |
| Get orders of customer 12345 | GET http://api.europcar.com/customers/12345/orders |
| Create an order line in the order 67890 pour the customer 12345 | POST http://api.europcar.com/customers/12345/orders/67890/lineitems |
| Get the first forder line in the order 67890 for the customer 12345 | GET http://api.europcar.com/customers/12345/orders/67890/lineitems/1 |

The examples below illustrate the concept of hierarchy to be applied and the traversability of the hierarchy via a unique ID that is interposed between the common names representatives the traversed entities.

 ### Practices to be avoided
 The first anti-pattern is to group REST services within a single URI and using the parameters of the query-string to distinguish services from one another. For example, the update  URI of a customer could be:

```
GET http://bad-api.europcar.com/services?op=update_customer&id=12345&format=json
````

The problem is that this URL is not self-described. 
1. neutral GET verb designates a transaction which is not. 
2. The URI has a hierarchical structure identical for all theresources. This does not allow to differentiate the resources accessed just through the reading of the URI. 
3. The format of the returned message should be indicated in the header (Header `` `Accept: application / json```). This information is independent from the context of the application, this information must be in the header.


Another wrong use is to mention the operation in the URI:
```
GET http://bad-api.europcar.com/customers/12345/create
```

The URL must designate a resource and not an operation on a resource. The URI must be identical regardless of the operation that is performed on the resource (POST / GET / PUT / DELETE)

### Pluriel versus singulier
![Tip](lightbulb1.png)

The rule to follow is this. When the resource refers to a collection of objects then we must use the plural.

But what happens when no resource actually exists on the server ? Consider that we wanted to build a service that verifies that the email referred respects a well-defined pattern. 

This is a neutral operation, the appropriate HTTP verb GET verb. 

In our case, there is no pattern entity in the database, we would be tempted to use the singular with type URI:
```
GET http://bad-api.europcar.com/pattern-valid
```

This violates the principle of hierarchy. One solution is to consider that there are a multitude of patterns, we can deduce that the pattern is a virtual entity to be designated by a name in the plural. To validate the email, we can consider that the identifier "email" is our pattern in this virtual collection patterns.



The URI could then be written::
```
GET http://api.europcar.com/patterns/email
```

The validity may be regarded as a property and thus will appear later in the hierarchy. The final URI becomes:

```
GET http://api.europcar.com/patterns/email/validity
```

Cette URI désignant de manière unique la ressource, la valeur saisie par le client sera transmise dans la query string. Cela nous donne l'appel suivant :

```
GET http://api.europcar.com/patterns/email/validity?value=name@domain.com
```
# exposition d'objets
Les objets peuvent être exposés au client comme une hiérarchie de composition ou d'association d'objets. C'est cette composition ou association qui doit permettre d'obtenir l'URI d'accès à la ressource.

Composition / aggregation : la hiérachie matérialise la relation entre une ressource est des sous-ressources qui la composent. Celles-ci ont le cycle de vie de la ressource principale et ne sont accessible que via cette dernière.

Association : cette relation matérialise la navigabilité entre des ressources autonomes (qui peuvent exister en dehors de cette relation). 


Considérons le schéma relationnel suivant :

![Rental Diagram](rent9.png)

- Une commande est le rapprochement d'une ou plusieurs lignes de commandes à un et un seul client.
- Un client peut n'avoir jamais passé de commande.
- Un produit peut n'avoir jamais été commandé.

Les structures d'association suivantes sont alors possibles :


|Type URI | Choix de conception |
| -- | -- |
|URI1 |![](rent7.png)|
|URI2 |![](rent10.png)|

Le choix de l'une ou de l'autre des modélisations est entièrement à l'appréciation de l'analyste métier et de la vue hiérarchique qu'il souhaite donner au système d'informations. Les URIs suivantes sont équivalentes :

|Verbe HTTP | URI1 | / URI2 |
| -- | -- | -- |
| Description |l'analyste métier impose uniquement la connaissance de la commande pour accéder aux lignes de commande de la dite commande.| Dans le second cas, l'accès aux lignes de commande impose de connaître également le client pour de la commande. |
| GET | /orders/12345/lineitems |/customers/6785/orders/12345/lineitems |

Dans les deux cas, rien n'empêche l'analyste métier de permettre l'accès aux commandes, clients ou lignes de commande directement via une clef d'accès unique. Là encore il s'agit d'un choix métier quant à l'exposition des ressources via l'API REST.

![Tip](lightbulb1.png)Lors de l'accès à un objet, on peut vouloir le discriminer par un attribut. Lorsque cet attribut identifie de manière unique dans la hiérarchie accédée la ressource, alors il fera partie de l'URI. 
L'accès à une sous-ressource sans identifiant peut être fait par une position / index 
Dans les autres cas, ce sera un critère de recherche. 

Dans l'exemple ci-dessous, on souhaite les commandes passée le 10/09/2015.

``` .../orders/20150910 ``` est à proscrire car plusieurs commandes peuvent avoir été  passées le 10/09/2015. Une requête correcte est la suivante : ``` .../orders?date=20150910``` et le retour sera une collection.

![Tip](lightbulb1.png)Composition versus association : On modélisera une composition lorsque l'appel REST renverra dans le flux la sous-ressource, et une association lorsque l'appel REST renvoie uniquement un identifiant technique et/ou fonctionnel accompagné d'un libellé (Cf. fragments externes / XF)

## exécution d'opérations métier
Dans les cas métiers (autres que CRUD), l'invocation d'un service REST renvoie généralement le résultat de l'exécution d'un acte métier. On peut citer les actions métiers suivantes :
- Signer le contrat
- Valider la location
- Notifier le client


Dans ces cas, on remplacera le verbe par le substantif adéquat comme le montre le tableau ci-dessous :


| Action | Exemple d'URI | Description|
| -- | -- | -- | -- |
| Signer la commande | POST /orders/12345/signature | Créer une signature et l'associer à la commande |
| Valider la commande | PUT /orders/12345/validation | Mettre à valide  du statut de la commande |
| Notifier le client | POST /customers/789067/notification | Création d'une notification et association au client |

Dans le tableau ci-dessus, la description indique la perception qu'a le client du service REST. Dans la réalité, les opérations qui ont lieu sur le S.I. sont beaucoup plus riches et complexes.

Note : suivant les cas métier, l'invocation du service sera accompagnée ou non d'un payload

L'usage de ces substantifs permet de conserver l'intention métier et le bon niveau d'abstraction des opérations

Le choix du verbe (PUT, GET , POST, DELETE) dépendra de l'idempotence et de la neutralité de l'action que l'on souhaite exécuter. 
