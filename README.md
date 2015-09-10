
## Objet

REST (Representational State Transfer) est un style d'architecture pour les systèmes hypermédia indépendant du protocole (HTTP /...) et du format (JSON/XML/ ...).
REST est défini par six contraintes décrites à l'origine par [Roy Fielding](http://api.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) qui définissent les bases d'une API REST.
Ces six contraintes sont :
- Interface uniforme
- Sans état
- Mise en cache
- Client / serveur
- Structuré en couches
- Exécution de code à la demande

Ce document est un guide du développeur qui regroupe les bonnes pratiques à respecter dans la cadre de la réalisation d'une architecture de services à base de services REST. Il se limite exclusivement au protocole HTTP et aux formats JSON/XML.


## Les six contraintes de REST
### Une interface uniforme
Cette contrainte se caractérise par quatre règles essentielles :
- Identifiation unique des ressources : Chaque ressource est identifiée de manière unique au travers d'URIs. Le serveur renvoie une représentation de la ressource au client dans le respect d'un schéma prédéfini et indépendant de la représentation physique de la ressource sur le serveur. Les formats les plus communs sont JSON et XML encodés en UTF-8.

### Sans état
L'approche sans état est clef dans REST. Elle signifie que l'ensemble des éléments requis à la manipulation des données est contenue dans l'URI, les paramètres de l'URI (query-string), le corps de la requête ou les entêtes. Une fois la requête traitée, le serveur renvoie au travers des entêtes, du corps ou du statut de la réponse , l'ensemble des informations requises pour d'éventuels appels ultérieurs.

Cela tranche avec l'approche classique qui consiste à stocker en session des informations qui resteront présentes d'une requête HTTP à une autre. Dans l'approche REST, le client doit inclure toutes les informations requises par le serveur pour exécuter la requête.
L'approche sans état offre une meilleure scalabilité des applications dans la mesure où le serveur n'a pas besoin de maintenir et/ou répliquer l'état de session. De plus, les composants de régulation de charge n'ont pas à se précoccuper de l'affinité de session qui complexifie les paramétrages d'infrastructure et le déploiement de nouvelles versions d'applications.

P.S. Il n'est pas toujours possible d'avoir une approche stateless. Des services tels que OAuth requiert le maintien d'informations de session. La compatibilité avec des versions antérieures de l'application sont également des raisons de maintien d'information de session.

Si le maintien d'une session est indispensable, il faut :
- retenir que dans la mesure du possible, les informations de session ne doivent pas servir comme maintien d'un état transitoire d'une requête HTTP à une autre mais uniquement comme un mécanisme de cache d'informations valide pendant toute la durée de la session.
- s'assurer que la session est disponible sur l'ensemble des serveurs de la ferme (absence d'affinité de session).

### Mise en cache
Les clients Web (Desktop et mobile) sont en mesure de mettre en cache les réponses. Les réponses doivent donc, implicitement ou explicitement, se définir comme étant candidates ou pas à être mises en cache pour éviter que le client ne fasse des requêtes serveurs inutiles ou utilise des informations obsolètes. Le bénéfice d'une bonne gestion de la mise en cache des données permet d'améliorer la scalabilité et la performance.

### Client / Serveur
Le client est à l'initiative de la requête.
Le client et le serveur communiquent au travers d'une interface normalisée indépendante de la représentation des données sur le serveur et de l'interface utilisateur côté client. Cela permet aux deux parties (client et serveur) d'évoluer indépendamment l'une de l'autre tant que l'interface d'échange est respectée ou reste compatible.

###Structuration en couches
Un client ne peut pas savoir s'il est directement connecté au serveur d'application cible ou s'il est servi par un serveur intermédiaire de la chaine de liaison. Ces serveurs jouent en général le rôle de régulateur de charge, de cache partagé ou de service d'authentification et d'habilitation.

###Exécution de code à la demande (Optionnel)
Cette contrainte est la seule optionnelle des six contraintes REST imposées. Elle permet de renvoyer au client du code qui sera exécuté sur le poste client pour étendre les fonctionnalités du serveur. Il peut s'agir d'applets Java ou de scripts javascript. Cela peut concerner notamment l'accès à des périphériques locaux.


## Les verbes HTTP
Les verbes HTTP permettent de classer les opérations de service REST en trois catégories :
- Les verbes idempotents
- Les verbes neutres
- Le verbe POST

### Idempotence
Un verbe est dit idempotent lorsque l'application du verbe à une URI produit le même résultat qu'on l'applique une ou plusieurs fois.
Les verbes PUT et DELETE (sauf dans certains cas pour ce dernier) sont dits idempotents

### Neutralité
Un verbe est dit neutre lorsqu'il ne conduit à aucun effet de bord sur le serveur.
Un verbe neutre est donc également idempotent.
Les verbes GET, HEAD, OPTIONS et TRACE sont des verbes neutres.

 ### Le verbe GET pour la lecture
La méthode HTTP GET est utilisée pour récupérer une ressource. En cas de succès, la méthode GET renvoit une représentation XML ou JSON de la ressource sollicitée et un code réponse HTTP 20X.
En cas d'erreur, cette méthode renvoie dans la plupart des cas un code réponse 404 (NOT FOUND) ou 400 (BAD REQUEST).

Exemples:
```
GET http://api.europcar.com/api/cars/12345 : Récupère le véhicule 12345
GET http://api.europcar.com/api/cars/12345/drivers : Récupère tous les drivers du véhicule 12345
```
Le verbe HTTP GET doit être utilisé exclusivement dans le cadre d'une opération neutre.

 ### Le verbe PUT pour la mise à jour
 La méthode PUT est utilisée dans le cadre de la mise à jour d'une ressource sur le serveur. Le corps de la requête contient alors au format JSON ou XML la représentation mise à jour de la ressource référencée.

La méthode PUT peut être également utilisée dans le cadre de la création d'une ressource mais uniquement lorsque le choix de l'identifiant de la ressource est de la responsabilité du client.

En cas de succès, le code réponse à renvoyer est le suivant :
- Code 200 s'il s'agit d'une mise à jour et que le corps de la réponse n'est pas vide
- Code 204 s'il s'agit d'une mise à jour et que le corps de la réponse est vide
- Code 201 s'il s'agit d'une création. Dans ce cas, le corps de la réponse peut être vide.

PUT n'est pas une opération neutre mais une opération idempotente, il faut donc veiller à ce que plusieurs appels à la méthode produisent le même effet.

Exemples :
```
PUT http://api.europcar.com/cars/12345 : Met à jour le véhicule 12345
PUT http://api.europcar.com/cars/12345/drivers/67890 : Met à jour le driver 67890 du véhicule 12345
```

 ### Le verbe POST pour la création
 Le verbe POST est souvent utilisé pour la création de nouvelles ressources. Le serveur est responsable de l'association d'un identifiant unique à la ressource.

Exemples:
```
POST http://api.europcar.com/cars : Crée un véhicule, les données associées au véhicule sont transmises dans le corps de la requête
POST http://api.europcar.com/cars/12345/drivers : Crée un driver pour le véhicule 12345.les données associées au driver sont transmises dans le corps de la requête.
```
En cas de succès, le code réponse HTTP 201 est renvoyé avec l'entête "Location" qui a pour valeur le lien qui référence directement la ressource ainsi créée.


 ### Pourquoi et quand Utiliser POST au lieu de PUT
 Utiliser PUT lorsque :
 - La ressource existe et doit être mise à jour
 - La ressource n'existe pas et le client est chargé de déterminer l'identifiant unique de la ressource

 Utiliser POST lorsque :
 - La ressource n'existe pas et qu'elle doit être créée avec un identifiant déterminé par le serveur
 - Lorsque l'opération n'est ni idempotente, ni neutre.


 ### Le verbe DELETE pour la suppression
Le verbe DELETE est utilisé pour supprimer la ressource identifiée par une URI.

Exemples:
```
DELETE http://api.europcar.com/cars/12345 : Supprime le véhicule 12345
DELETE http://api.europcar.com/cars/12345/drivers : Supprime tous les drivers du véhicule 12345
```

Selon la spécification HTTP, la méthode DELETE est idempotente car la suppression d'une ressource plusieurs fois d'affilée produit le même résultat, la ressource n'étant plus là.

### Codes réponses des verbes HTTP
Le tableau ci-dessous dresse la liste des codes HTTP qu'il est recommandé de renvoyer quand l'opération s'applique à une liste ou une seule ressource.

|Verbe HTTP | /cars | /cars/{id} |
| -- | -- | -- |
| GET | 200 5OK), renvoi d'une liste de véhicules.| 200 (OK), renvoi d'un seul véhicule. 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide. |
| PUT | 404 (NOT FOUND) | 200 (OK) ou 204 (NO CONTNTENT). 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide. |
| POST | 201 (CREATED), avec l'entête "Location" valorisé avec le client /cars/{id} contenant le nouvel identifiant | 404 (NOT FOUND) |
| DELETE | 404 (NOT FOUND), sauf si l'on souhaite supprimer toute la collection. | 200 (OK). 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide.   |


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


## HATEOAS ( à passer en annexe)
### Le principe
### Les règles minimales à respecter
### Comment les consommer

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


## Tri
Les options de tri sont placées dans la query-string. Le paramètre de tri est nommé "sort" et les critères de tri sont placés dans l'ordre séparés par une barre verticale "|". Lorsque le tri su un critère est descendant, il doit être  préfixé par le caractère "-".
Dans l'exemple ci-dessous, on renvoie la liste des véhicules triés par marque acendante et par date demise en service descendante:

```
GET http://api.europcar.com/cars?sort=brand|-date_of_service
```


## La réponse du service REST
La structure de réponse d'un appel de service REST doit être la suivante :

- Un code réponse HTTP
- Un statut
- Un message
- Les données.

### Le code réponse HTTP
Il s'agit d'un entier indiquant le code réponse HTTP. Ce code pourra également être inclus dans le corps de la réponse sous la forme d'un attribut

### l'attribut status
Cet attribut contient un des 3 textes suivants :
- "fail" pour les codes réponse HTTP 5XX
- "error" pour les codes réponse HTTP 4XX
- "success" pour les codes réponse HTTP 1XX, 2XX et 3XX.

### l'attribut message
Cet attribut est exclusivement présent lorsque le statut est "fail" ou "error". Il contient la liste des messages d'erreur à prendre en compte par le client.

### l'attribut data
Contient le corps de la réponse. En cas d'erreur (status égal à "fail" ou "error") il contient la liste des exceptions remontées.

Exemple 1:
```
{
"code":200,
"status":"success",
"data": [{"name":"Vincent"}]
}```


Exemple 2:
```
{
"code":400,
"status":"error",
"message" : ["Parameter ABC should be a valid date", "Unknown Field XYZ"]
"data": ["InvalidArgumentException", "UnknownFieldException"]
}```



## Versioning de services
### En utilisant la négotiation de contenu
Cette approhe est adaptée lorsque la version du service n'a pas changé mais qu'il existe plusieurs représentations possibles d'une ressource.
Dans ce cas, l'entête HTTP "Accept" est utilisé. Dans l'exemple suivant, on demande la ressource au format JSON en version 1.

```
Accept: application/json; version=1
```

La réponse renvoie alors dans l'entête HTTP "Content-Type" le format et la version retournée. Dans l'exemple suivant, la ressource est renvoyée au format JSON en version 1.
```
Content-Type: application/json; version=1
```

Si le client ne précise aucune version, le serveur doit renvoyer la plus ancienne version supportée par la ressource. Cette règle s'applique également lors de la création ou de la mise à jour d'une ressource. Les données fournies doivent être considérées comme étant dans la plus ancienne représentation supportée.

Lorsque la version ou le format demandé n'existent pas ou ne sont plus supportés, le code réponse HTTP 406 (NOT ACCEPTABLE) doit être renvoyé.

### En utilisant l'URI
Dans ce cas on préfixe l'URI par un numéro de version vXXX ou XXX désigne le numéro de version. Dans l'exemple ci-dessous on adresse un service dans sa version 2:
```
GET http://api.europcar.com/v2/customers/12345
```

Cela permet de placer des règles de routage d'URL assez facilement au niveau des serveurs HTTP intermédiaires.

### Conditions à la création d'une nouvelle version de l'API?
Dès que la compatibilité ascendante est cassée, il faut impérativement créer une nouvelle version. Cela concerne les cas suivants notamment :

- Renommage d'un attribut ou d'une entité
- La suppression d'un attribut ou d'une entité
- La modification du type d'un attribut
- La modification des règles de validation d'un attribut ou d'une entité

Par contre, dans les cas ci-dessous, il n'est pas nécessaire de créer une nouvelle version de l'API:

- Ajout de nouveaus attributs à la réponse
- Support de nouveaux formats
- Support de nouvelles langues

### Le support de la dépréciation d'un service
Si un service est déprécié mais qu'il contitnue d'être supporté, il faut l'indiquer dans l'entête "Deprecated" qui est de type boolean:
```
Deprecated: true
```

## Sécurisation des services
 ### Authentification
   ####  Authentification de niveau partenaire (token)
   ####  Authentification de niveau user (user/password)
   ####  Authentification interne (token)
 ### Habilitation
   - Il ne faut pas oublier d'alerter sur le fait que les roles doivent être décrits par l'équipe fonctionnelle.
   - Inclure JWT ?
   -

## La gestion du cache

Le cache est mis en oeuvre exclusivement pour les appels de type GET. Les entêtes suivants permettent de supporter les mécanismes de cache:

| Entête | Description | Exemple |
| -- | -- | -- |
| Date | Date et heure à laquelle la ressource a été renvoyée (au format RFC 1123) | Mon, 3 Aug 2015 09:26:12 GMT |
| Cache-Control | Le nombre de secondes maximum pendant lesquelles la réponse peut être mise en cache. La valeur no-cache indique que la répons ene doit pas être mise en cache. | Cache-Control: 3600 ou Cache-Control: no-cache |
| Expires | Date absolue au format RFC 1123 à laquelle la ressource expire.  | Mon, 3 Aug 2015 09:26:12 GMT |
| Pragma | Si la ressource ne doit pas être mise en cache, alors il faut rajouter cet entête avec la valeur no-cache | Pragma: no-cache |
| Last-Modified | Date au format  RFC 1123 de dernière mise à jour de la ressource | Mon, 3 Aug 2015 09:26:12 GMT |


##Gestion des sessions
### Passage d'un identifiant technique pour les sessions
### Passage d'un identifiant visiteur

## Les codes erreur HTTP et leur utilisation
La liste exhaustive des codes réponse HTTP REST est disponible à cette adresse : [restapitutorial](http://www.restapitutorial.com/httpstatuscodes.html)

Les 10 principaux codes HTTP REST sont les suivants.


| Code | Libellé | Description |
| -- | -- | -- |
| 200 | OK | Code succès générique |
| 201 | CREATED | Création avec succès via POST ou PUT avec mise à jour de l'entête Location qui référence la ressource ainsi créée. |
| 204 | NO CONTENT | Quand le corps de la réponse est vide et que l'opération s'est exécutée avec succès. |
| 304 | NOT MODIFIED | En réponse à la méthode GET conditionnelle pour indiquer que la ressource n'a pas été modifiée depuis la date indiquée en entête |
| 400 | BAD REQUEST | Paramètres de la requête invalides |
| 401 | UNAUTHORIZED | Absence du tocken d'authentification dans la requête |
| 403 | FORBIDDEN | Utilisateur on autorisé à accéder à la ressource |
| 404 | NOT FOUND | Quand la ressource demandée n'a pas été trouvée. |
| 409 | CONFLICT | Quand la requête risque de créer des conflits tel que des doublons en création ou des suppressions en cascade non supportées |
| 500 | INTERNAL SERVER ERROR | Erreur générique côté serveur |

## Concevoir et documenter son API REST
 ### Le modèle de données
 ### les opérations
 ### Les services
 ### Avec UML
 ### Avec Swagger
 ### Avantages / incovénients des approches



