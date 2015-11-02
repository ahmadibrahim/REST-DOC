### Representation of data in Input / Output

#### Objets

- When an object is returned, it must be self-described. This means that one will find all the criteria that enabled to return the object in the object returned back if they are part of the returned object. So when accessed by ID, the ID will also be included in the returned object. ``` / Orders/12345 ``` will return a command as follows:
 

``` 
{
 "id":"12345", // id is included in the response despite being in the request URI
  ...
} ```


- the returned object is never named. If it is a JSON structured type, it will appear with the attributes framed by an opening brace and a closing brace, as is the case in the example above. No attribute must prefix the returned object.


- Exposure of a resource must ignore its real representation in the information system (see previous example about pattern). If there is a resource resulting from the aggregation of a number of other resources, then it must be seen as an compound object .

![Tip](lightbulb1.png) When there is a composition relation (UML sense), the whole object(or a view of it) must be returned.

When there is a association relation( UML sense), You should return the minimum namely id and functional name for example.


#### Vues d'un objet

![Tip](lightbulb1.png) When it is desired to access a particular view of a resource, for example the client identity or retrieve only its payment data. You need to  prefix the view that you want to access by the word ```views```:

```
/customers/12345/views/ident
/customers/12345/views/payment
```

The absence of the prefix ```views``` means that we want the default view: ``` /customers/12345 ```.



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
GET http://api.europcar.com/orders/1234/linetitems/views/names
```
pourra produire la réponse suivante :
``` 
[
    {
    "id":"12345",
    "name": "ABCF DEF"
    },
    {"id":"57643",
    "name": "XYZ UVW"
    }
]
```


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




