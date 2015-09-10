
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


Le choix du verbe (PUT, GET , POST, DELETE) dépendra de l'idempotence et de la neutralité de l'action que l'on souhaite exécuter. 




