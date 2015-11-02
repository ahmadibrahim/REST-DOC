## Sort
Sort options are placed in the query-string. 

![Tip](lightbulb1.png) The sort parameter is called "sort" and sort criteria are placed in the right order separated by a vertical bar "|". When sorting a known criterion in a descendant way, it must be prefixed by the "-" character. In the example below, it returns a list of vehicles ordered by ascending brand and descending by date of commissioning:
Les options de tri sont placées dans la query-string. 

![Tip](lightbulb1.png)Le paramètre de tri est nommé "sort" et les critères de tri sont placés dans l'ordre séparés par une barre verticale "|". Lorsque le tri su un critère est descendant, il doit être  préfixé par le caractère "-".

Dans l'exemple ci-dessous, on renvoie la liste des véhicules triés par marque ascendante et par date de mise en service descendante:

```
GET http://api.europcar.com/cars?sort=brand|-date_of_service
```
Les attributs sur lesquels le tri peut être opéré doivent être indiqués dans la documentation de l'API
