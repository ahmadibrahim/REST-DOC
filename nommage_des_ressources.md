### Représentation des données en entrée / sortie
#### Objets

Les objets sont exposés au client comme une hiérarchie de composition d'objets. C'est cette composition qui doit permettre d'obtenir l'URI d'accès à la ressource.

Considérons le schéma relationnel suivant :

![Rental Diagram](rent2.png)

- Une location est le rapprochement d'un ou plusieurs drivers à un et un seul véhicule.
- Un Driver peut n'avoir jamais loué de véhicule
- Un véhicle peut ne jamais avoir été loué.

Les structures de composition suivantes sont alors possibles :

![](rent3.png)
![](rent5.png)

Le choix de l'une ou de l'autre des modélisations est entièrement à l'appréciation de l'analyste métier et de la vue hiérarcchique qu'il souhaite donner au système d'informations. Les URIs suivantes sont équivalentes :

|Verbe HTTP | URI1 | / URI2 |
| -- | -- | -- |
| GET | 200 (OK), renvoi d'une liste de véhicules.| 200 (OK), renvoi d'un seul véhicule. 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide. |
| PUT | 404 (NOT FOUND) | 200 (OK) ou 204 (NO CONTNTENT). 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide. |
| POST | 201 (CREATED), avec l'entête "Location" valorisé avec le client /cars/{id} contenant le nouvel identifiant | 404 (NOT FOUND) |
| DELETE | 404 (NOT FOUND), sauf si l'on souhaite supprimer toute la collection. | 200 (OK). 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide.   |



#### Collections
#### Les intervalles
#### Les règles de nommage
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

