## Authentification pour l'accès aux services
Une authentification est nécessaire soit au niveau de l'utilisateur soit au niveau du tiers appelant (le partenaire).

Dans le premier cas, l'utilisateur s'appuie en général sur un couple identifiant/mot de passe. Dans le second, il s'agit d'un token qui permet d'identifier le tiers. Dans les deux cas, il est préconisé d'utiliser l'entête [Basic Auth HTTP](http://tools.ietf.org/html/rfc2617).

```Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==```



####  Authentification de niveau partenaire (token)

   ####  Authentification de niveau user (user/password)
   ####  Authentification interne (token)
 ### Habilitation
   - Il ne faut pas oublier d'alerter sur le fait que les roles doivent être décrits par l'équipe fonctionnelle.
   - Inclure JWT ?
   -
