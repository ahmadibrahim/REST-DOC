## La gestion du cache

Le cache est mis en oeuvre exclusivement pour les appels de type GET. Les entêtes suivants permettent de supporter les mécanismes de cache:

| Entête | Description | Exemple |
| -- | -- | -- |
| Date | Date et heure à laquelle la ressource a été renvoyée (au format RFC 1123) | Mon, 3 Aug 2015 09:26:12 GMT |
| Cache-Control | Le nombre de secondes maximum pendant lesquelles la réponse peut être mise en cache. La valeur no-cache indique que la réponse ne doit pas être mise en cache. | Cache-Control: 3600 ou Cache-Control: no-cache |
| Expires | Date absolue au format RFC 1123 à laquelle la ressource expire.  | Mon, 3 Aug 2015 09:26:12 GMT |
| Pragma | Si la ressource ne doit pas être mise en cache, alors il faut rajouter cet entête avec la valeur no-cache | Pragma: no-cache |
| Last-Modified | Date au format  RFC 1123 de dernière mise à jour de la ressource | Mon, 3 Aug 2015 09:26:12 GMT |


![Tip](lightbulb1.png)Les règles de cache sont souvent exprimées au niveau des éléments d'infrastruture. Ils ne doivent généralement pas être positionnés par l'applicatif.