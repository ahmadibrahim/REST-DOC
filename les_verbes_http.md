## HTTP methods
HTTP methods (also called verbs) allow us to classify REST services into three categories :
- idempotent verbs
- neutral verbs
- The POST verb

### Idempotence
A verb is said to be idempotent when the application of this verb to the URI produces the same result whether applied once or many times.

![Tip](lightbulb1.png) The PUT and DELETE (except in certain cases for this last one) verbs are idempotent


### Neutrality
A verb is said to be neutral when it does not produce any side effect on the server.
A neutral verb is thus also idempotent

![Tip](lightbulb1.png) The verbs GET, HEAD, OPTIONS and TRACE are neutral verbs.

 ### The GET verb for reading
The GET method is used to get a resource. When it succeed, this method returns a JSON/XML representation of the resources and a HTTP Code 20X.
In case of failure, this method returns in most cases a 404 (NOT FOUND) of 400 (BAD REQUEST) response code.

Examples:
```
GET http://api.europcar.com/api/cars/12345 : return vehicle 12345
GET http://api.europcar.com/api/cars/12345/drivers : returns all drivers of vehicle 12345
```

The  HTTP GET verb should be used exclusively for neutral operations.


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
 ![Tip](lightbulb1.png)Utiliser PUT lorsque :
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
| GET | 200 (OK), renvoi d'une liste de véhicules.| 200 (OK), renvoi d'un seul véhicule. 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide. |
| PUT | 404 (NOT FOUND) | 200 (OK) ou 204 (NO CONTNTENT). 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide. |
| POST | 201 (CREATED), avec l'entête "Location" valorisé avec le client /cars/{id} contenant le nouvel identifiant | 404 (NOT FOUND) |
| DELETE | 404 (NOT FOUND), sauf si l'on souhaite supprimer toute la collection. | 200 (OK). 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide.   |

Les verbes utilisés dans le contexte d'Europcar sont : GET, POST, PUT, DELETE

