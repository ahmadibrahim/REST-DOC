## La réponse du service REST
La structure de réponse d'un appel de service REST doit être la suivante :

- Un code réponse HTTP
- Un statut
- Un message
- Les données.

### Le code réponse HTTP
Il s'agit d'un entier indiquant le code réponse HTTP. Ce code pourra également être inclus dans le corps de la réponse sous la forme d'un attribut

### l'attribut status
Cet attribut contient un des 3 textes suivants :
- "fail" pour les codes réponse HTTP 5XX
- "error" pour les codes réponse HTTP 4XX
- "success" pour les codes réponse HTTP 1XX, 2XX et 3XX.

### l'attribut message
Cet attribut est exclusivement présent lorsque le statut est "fail" ou "error". Il contient la liste des messages d'erreur à prendre en compte par le client.

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
"message" : ["Parameter ABC should be a valid date", "Unknown Field XYZ"]
"data": ["InvalidArgumentException", "UnknownFieldException"]
}```


** todo Pour chaque code erreur http décrire les erro code et messages possibles.