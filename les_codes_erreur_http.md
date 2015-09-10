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