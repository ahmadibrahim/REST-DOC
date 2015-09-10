
## exécution d'opérations
Dans les cas métiers (autres que CRUD), l'invocation d'un service REST renvoie généralement le résultat de l'exécution d'un acte métier. On peut les actions métiers suivantes :
- Signer le contrat
- Valider la location
- Notifier le client


Dans ces cas, on remplacera le verbe par le substantif adéquat comme le montre le tableau ci-dessous :



|Verbe HTTP | /cars | /cars/{id} |
| -- | -- | -- |
| GET | 200 (OK), renvoi d'une liste de véhicules.| 200 (OK), renvoi d'un seul véhicule. 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide. |
| PUT | 404 (NOT FOUND) | 200 (OK) ou 204 (NO CONTNTENT). 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide. |
| POST | 201 (CREATED), avec l'entête "Location" valorisé avec le client /cars/{id} contenant le nouvel identifiant | 404 (NOT FOUND) |
| DELETE | 404 (NOT FOUND), sauf si l'on souhaite supprimer toute la collection. | 200 (OK). 404 (NOT FOUND) si l'identifiant n'est pas trouvé ou est invalide.   |


Le choix du verbe (PUT, GET , POST, DELETE) dépendra de l'idempotence et de la neutralité de l'action que l'on souhaite exécuter. 




