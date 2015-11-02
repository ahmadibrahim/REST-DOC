## La réponse du service REST
The response structure of a REST service call must be:
- An HTTP response code 
- A status 
- A message 
- The data.

### The HTTP response code
Il s'agit d'un entier indiquant le code réponse HTTP. Ce code pourra également être inclus dans le corps de la réponse sous la forme d'un attribut

### l'attribut status
Cet attribut contient un des 3 textes suivants :
- "fail" pour les codes réponse HTTP 5XX
- "error" pour les codes réponse HTTP 4XX
- "success" pour les codes réponse HTTP 1XX, 2XX et 3XX.

### l'attribut message
Cet attribut est exclusivement présent lorsque le statut est "fail" ou "error". Il contient la liste des messages d'erreur à prendre en compte par le client.

Un message est un objet avec deux attributs ```code``` et ```description```.

Dans certains cas à la marge, il peut être utilise d'avoir des messages plus détaillés, auquel cas, il conviendra d'inclure un attribut ```details``` qui contient la liste des messages d'erreur détaillés.

### l'attribut data
Contient le corps de la réponse. En cas d'erreur (status égal à "fail" ou "error") il contient la liste des exceptions remontées.

Exemple 1:
```
{
"code":200,
"status":"success",
"data": [{"name":"Vincent"}]
}```


Exemple 2:
```
{
"code":400,
"status":"error",
"message" : 
    {
        "code": UserAbsent",
        "description" : "The request is invalid. Rent no more present"
    }
"details" : 
    [
        { 
            "code":"InvalidArgument", 
            "description" :"Parameter ABC should be a valid date"
        }, 
        { 
            "code":"UnknownField", 
            "description" :"Unknown Field XYZ"
        }
    ]
}```

