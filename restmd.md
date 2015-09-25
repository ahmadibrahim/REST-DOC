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
L'approche REST permet de bénéficier des mécanismes standards de gestion du cache du web

### Client / Serveur
Le client est à l'initiative de la requête.
Le client et le serveur communiquent au travers d'une interface normalisée indépendante de la représentation des données sur le serveur et de l'interface utilisateur côté client. Cela permet aux deux parties (client et serveur) d'évoluer indépendamment l'une de l'autre tant que l'interface d'échange est respectée ou reste compatible.

###Structuration en couches
Un client ne peut pas savoir s'il est directement connecté au serveur d'application cible ou s'il est servi par un serveur intermédiaire de la chaine de liaison. Ces serveurs jouent en général le rôle de régulateur de charge, de cache partagé ou de service d'authentification et d'habilitation.

###Exécution de code à la demande (Optionnel)
Cette contrainte est la seule optionnelle des six contraintes REST imposées. Elle permet de renvoyer au client du code qui sera exécuté sur le poste client pour étendre les fonctionnalités du serveur. Il peut s'agir d'applets Java ou de scripts javascript. Cela peut concerner notamment l'accès à des périphériques locaux.
Note : Europcar n'a pas prévu d'utiliser cette approche



## Concepts
- Une ressource est accédée au travers d'une URI.
- Une URI peut être invoquée via une ou plusieurs méthodes (GET / PUT / POST / DELETE) en fonction de l'opération que l'on souhaite exécuter (Lecture / Mise à jour / Création / Suppression).
- Une invocation de service REST peut inclure dans le corps de la requête des paramètres en entrée représentant un objet métier et recupérer dans la réponse une représentation de l'objet métier dans le flux de retour.
- Plusieurs variantes d'une représentation sont possibles. En général on retrouve les variantes JSON et XML.

Le diagramme ci-dessous illustre ces concepts.

![Concepts REST](rest-concepts.png)
=> ToDo : indiquer la source du schéma 

