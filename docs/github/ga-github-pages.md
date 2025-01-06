---
tags:
  - Github Action
  - Github Pages
---

# Utiliser les Github pages

Lorsque vous avez un site statique à déployer, le plus simple peut être d'utiliser les github pages, ce système permet de publier simplement des sites web directement depuis votre dépôt.

Pour que cela fonctionne il faut tout de même mettre en jeu deux mécanismes :

- upload votre site statique dans un artifact (sorte d'espace de stockage)
- activer la github page avec le contenu de votre artifact

Ces deux actions sont en réalité des actions Github action disponible dans le marketplace, il suffit donc d'écrire un workflow Github Action.


