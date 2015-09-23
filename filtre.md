## Filtrage


Les options de filtrage sont placés dans la query-string uniquement sous la forme d'un paramètre supplémentaire : "filter".
Le nom du critère est séparé de sa valeur par le chaine de caractères "::" et les critères de filtre sont séparés les uns des autres par le caractère '|'.

Ainsi, l'URI suivante pourrait permettre de récupérer les clients  avec une commande d'un montant supérieur  1000€ et avec un seul article:

```
GET http://api.europcar.com/orders?filter=amount:gt:1000|count::1
```

On peut complémenter les critères de filtres par des intervalles temporels  en ajoutant les paramètres "before" et "after". Ainsi dans l'exemple suivant on obtient les commandes  précédentes entre le 1er janvier 2015 et le 15 janvier 2015.

```
GET http://api.europcar.com/orders?filter=amount:gt:1000|count::1&after=timestamp&before=timestamp
```

Lorsque l'on souhaite des opérateurs plus riches, il faut alors définir une syntaxe dédiée. Pour l'inégalité on pourra alors utiliser la syntaxe suivante :

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



