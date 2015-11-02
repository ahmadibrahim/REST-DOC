## The six constraints of REST
### Uniform Interface

This constraint is defined by the fact that each resource is uniquely identified through a URI. The server returns a representation of the resource to the client with respect to a predefined schema that is independent of the physical representation of the resource on the server. The most common formats are JSON and XML encoded as UTF-8 entities.

### Stateless
Stateless approach is key to the REST architecture style. It implies that all the data required to service the request is contained within the URI, the URI parameters (query-string), the body of the request and/or the headers. Once the request is serviced, the server returns the exhaustive information required to submit future requests through the headers, the body and the response status code.



This is a breaking change with the classical approach where data is stored in a server side session store and made available to any future requests from the same user in the same session. REST requires that each client includes all the data required by the server to execute the request.

The stateless approach brings more scalability to applications since the server does not need to maintain and/or replicate the session state. Moreover, load balancing components do not have to be aware of the session affinity that are usually complex to implement at the infrastructure level and makes deployment of a new version of the application less smooth since it requires all sessions to be shutdown first.


P.S. Nonetheless, it is not always possible to have a stateless approcach . Services like OAuth require information to be maintained in the session store. Compatibility with previous versions of the application may also be a motivation to maintain session data accross requests

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

Note : Europcar n'a pas prévu d'utiliser l'exécution de code à la demande.



## Concepts
- En REST, l'abstraction clef désignant l'unité d'information est la ressource. Tout information qui peut être nommée est potentiellement est une ressource. Plus généralement, tout concept qui peut être désigné par une référence hypertexte doit être considéré comme une ressource.
- Une ressource est accédée au travers d'une URI.
- Une URI peut être invoquée via une ou plusieurs méthodes (GET / PUT / POST / DELETE) en fonction de l'opération que l'on souhaite exécuter (Lecture / Mise à jour / Création / Suppression).
- Une invocation de service REST peut inclure dans le corps de la requête des paramètres en entrée représentant un objet métier et recupérer dans la réponse une représentation de l'objet métier dans le flux de retour.
- Plusieurs variantes d'une représentation sont possibles. En général on retrouve les variantes JSON et XML.

Le diagramme ci-dessous illustre ces concepts.

![Concepts REST](rest-concepts.png)

Figure : http://www.apigee.com

