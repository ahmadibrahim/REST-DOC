##Gestion des sessions
ST dans REST signifie State Transfer, ce qui signifie que les informations de session doivent être transférées au client et non stockées sous la forme d'objets en session.

La règle est la suivante :
- Toute état spécifique à la session en cours dans être stockée côté client.
- Tout état spécifique à un objet métier est géré côté serveur.







### Passage d'un identifiant technique pour les sessions
### Passage d'un identifiant visiteur