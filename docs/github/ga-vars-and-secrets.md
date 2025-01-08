---
tags:
  - Github Action

---
# Variable et secrets dans Github Action

Github Action propose un système de gestion de variable et de secrets pour enrichir l'exécution et améliorer l'adaptabilité de vos workflows.

La définition se passe directement dans l'interface Github dans cette section :

![alt text](image-4.png)

Il y a deux onglets, un pour la gestion des secrets et l'autre pour la gestion des variables.

Les variables sont accessible dans les workflow via deux type d'appels, par exemple :

- `{{ vars.VAR1 }}` accès aux variables classique non chiffrées.
- `{{ secrets.SECRET1 }}` accès aux variable de type secrets.

Ces variables peuvent être utilisées de deux manière différentes :

- directement en argument aux actions Github via les options `with:`
- en les passant comme des variable d'environnement aux commandes sous jacentes

Ainsi la step ci dessous pourra donc afficher le contenu de la variable `MSG` :

```yaml
- run: echo $MSG
env:
    MSG: ${{ vars.MSG }}
```

## Cas pratique d'usage

Vous pouvez stocker directement le contenu d'un fichier .env de producation dans par exemple une variable qu'on nommerait `ENV`.

```env
DB_HOST=mydb.oclock.io
DB_USER=appuser
DB_PASSWORD=4g5sz6rst5h14dhrtyh5fqesr4g44
DEBUG=false
```

## Utilisation des environements

Les environments contextualisent les variable selon des contextes, ainsi la variable ENV dans le contexte `PROD` va donner le contenu du fichier .env pour la production et la même variable `DEV` contenir les variables du contexte de pré-production.

TODO: à développer
