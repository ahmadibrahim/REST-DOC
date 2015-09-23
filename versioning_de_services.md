## Versioning de services
### En utilisant la négotiation de contenu
Cette approhe est adaptée lorsque la version du service n'a pas changé mais qu'il existe plusieurs représentations possibles d'une ressource.
Dans ce cas, l'entête HTTP "Accept" est utilisé. Dans l'exemple suivant, on demande la ressource au format JSON en version 1.

```
Accept: application/json; version=1
```

La réponse renvoie alors dans l'entête HTTP "Content-Type" le format et la version retournée. Dans l'exemple suivant, la ressource est renvoyée au format JSON en version 1.
```
Content-Type: application/json; version=1
```

Si le client ne précise aucune version, le serveur doit renvoyer la plus ancienne version supportée par la ressource. Cette règle s'applique également lors de la création ou de la mise à jour d'une ressource. Les données fournies doivent être considérées comme étant dans la plus ancienne représentation supportée.

Lorsque la version ou le format demandé n'existent pas ou ne sont plus supportés, le code réponse HTTP 406 (NOT ACCEPTABLE) doit être renvoyé.

### En utilisant l'URI
Dans ce cas on préfixe l'URI par un numéro de version vXXX ou XXX désigne le numéro de version. Dans l'exemple ci-dessous on adresse un service dans sa version 2:
```
GET http://api.europcar.com/v2/customers/12345
```

Cela permet de placer des règles de routage d'URL assez facilement au niveau des serveurs HTTP intermédiaires.
![Tip](lightbulb1.png) Le versioning via l'URI est à privilégier. 

### Conditions à la création d'une nouvelle version de l'API?
Dès que la compatibilité ascendante est cassée, il faut impérativement créer une nouvelle version. Cela concerne les cas suivants notamment :

- Renommage d'un attribut ou d'une entité
- La suppression d'un attribut ou d'une entité
- La modification du type d'un attribut
- La modification des règles de validation d'un attribut ou d'une entité qui rompt la compatibilité avec l'existant. 

Par contre, dans les cas ci-dessous, il n'est pas nécessaire de créer une nouvelle version de l'API:

- Ajout de nouveaus attributs à la réponse
- Support de nouveaux formats
- Support de nouvelles langues

### Le support de la dépréciation d'un service
Si un service est déprécié mais qu'il contitnue d'être supporté, il faut l'indiquer dans l'entête "Deprecated" qui est de type boolean:
```
Deprecated: true
```
