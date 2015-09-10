##Gestion des sessions
ST dans REST signifie State Transfer, ce qui signifie que les informations de session doivent être transférées au client et non stockées sous la forme d'objets en session. La règle est la suivante :
- Toute état spécifique à la session en cours dans être stockée côté client.
- Tout état spécifique à un objet métier est géré côté serveur.

La session peut être utile dans cartains cas à la marge :
- Compatibilité avec une application antérieure
- Préchargement de certaines ressources métiers volumineuses utiles au traitement

Dans ce cas, un identifiant de session est transmis au client au travres d'un cookie dans l'entête HTTP.

```
Set-Cookie: jsessionid=ig2fac55; path=/; secure; HttpOnly
```

La présence des deux indicateurs ```secure```et ```HttpOnly``` permet de sécuriser le cookie de la mnière suivante :

- L'indicateur ```secure``` empêche le cookie d'être envoyé en clair sur le canal HTTP et force l'utilisation du protocole HTTPS.
- L'indicateur ```HttpOnly```empêche le code JavaScript d'accéder au cookie, pratique courante dans les attaques de type XSS (Cross site scripting). 

