
## exécution d'opérations
Dans les cas métiers (autres que CRUD), l'invocation d'un service REST renvoie généralement le résultat de l'exécution d'un acte métier. On peut les actions métiers suivantes :
- Signer le contrat
- Valider la location
- Notifier le client


Dans ces cas, on remplacera le verbe par le substantif adéquat comme le montre le tableau ci-dessous :


Le choix du verbe (PUT, GET , POST, DELETE) dépendra de l'idempotence et de la neutralité de l'action que l'on souhaite exécuter. 




