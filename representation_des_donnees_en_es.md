### Représentation des données en entrée / sortie

Note : dans le contexte d'Europcar, on ne traite que de la gestion d'échanges JSON
Note : ajouter un paragraphe sur le nommage des attributs (notamment, usage du camelCase)

#### Objets

- Lorsqu'un objet est renvoyé, il doit être auto-décrit. On retrouvera tous les critères ayant permis 
de renvoyer l'objet dans l'objet renvoyé en retour s'ils font partie de l'objet renvoyé. Ainsi lors d'un accès par ID, l'ID devra être également inclus dans l'objet retourné. ``` /orders/12345 ``` renverra une commande comme suit :

ToDo : préciser la notion de "auto décrit"

``` 
{
 "id":"12345", // id est repris dans la réponse bien qu'il soit dans l'URI de la requête
  ...
} ```


- L'objet renvoyé n'est jamais nommé. S'il s'agit d'un type structuré JSON, il se présentera avec les attributs encadrés par une accolade ouvrante et une accolade fermante comme cela est le cas dans l'exemple ci-dessus. Aucun attribut ne doit préfixer l'objet renvoyé.


- L'exposition d'une ressource fait fi de sa représentation réelle dans le système d'information (cf. exemple pattern précédent). S'il s'agit d'une ressource résultant de l'agrégation de plusieurs autres ressources, alors il elle doit être perçue comme étant un objet composé.

![Tip](lightbulb1.png)Dans les relations de composition, c'est l'objet tout entier (ou une vue sur celui-ci) qui doit être renvoyé. Dans les relations d'association, on renvoie  le minimum à savoir l'id et le nom fonctionnel par exemple.


### Vues d'un objet
![Tip](lightbulb1.png)Lorsque l'on souhaite accéder à une vue particulière d'une ressource, par exemple pour le client récupérer uniquement son identité ou uniquement ses données de paiement. On préfixera la vue que l'on souhaite accéder par ```views```:

```
/customers/12345/views/ident
/customers/12345/views/payment
```

L'absence du préfixe ```views```signifie que l'on souhaite la vue par défaut:
```/customers/12345```.

ToDo : aligner la gestion des views avec les cas de recherche (Filtres)


#### Collections
Les objets sont renvoyées comme une liste homogène de données, encadrées par un crochet ouvran tet un crocher ouvrant. Tout comme précédemment, les listes sont des objets et ne doivent donc jamais être nommées. Ci-dessous un exemple de liste avec deux éléments :
``` 
[
    {
        "id":"12345",
        "firstName": "ABC",
        "lastName":"DEF"
        ...
    },
    {
        "id":"57643",
        "firstName": "XYZ",
        "lastName":"UVW"
        ...
    }
]
```

Une URI qui renvoie une liste est de préférences terminée par un segment au pluriel comme dans l'exemple suivant :
```
GET http://api.europcar.com/orders/1234/linetitems
```

Cet appel renvoie la vue par défaut. Pour envoyer une vue spécifique, on pourra suffixer par ```views/nomdelavue```. Dans l'exemple ci-dessous on souhaite restituer uniquement les ids des lignes de commande

```
GET http://api.europcar.com/orders/1234/linetitems/views/ids
```
pourra produire la réponse suivante :
``` 
[
    {"id":"12345"},
    {"id":"57643"},
]
```

ToDo : faire une illustration ou on remonte une vue avec plusieurs attributs / fragments  et donc un tableau d'objets


#### Les dates
Les dates sont transmises de deux manières différentes selon qu'elles soient précisées dans un entête HTTP ou dans le corps de la requête / réponse.

Dans l'entête HTTP, les dates doivent respecter la RFC 1123 qui consiste à transmettre la date au format suivant :
````
Mon, 3 Aug 2015 09:26:12 GMT
````
Cette date comme on peut le voir ne permet pas d'avoir accès aux millisecondes. La pattern Java à appliquer pour l'obtenir est
```
EEE, dd MMM yyyy HH:mm:ss 'GMT'
```

Dans le corps de la requête ou de la réponse, les dates sont envoyées au format ISO8601 :
```
yyyy-MM-dd'T'HH:mm:ss.SSS'Z'
```

Pour une date sans heur, il faut quand même transmettre l'information horaire qui pourra être ignorée par l'applicatif.




