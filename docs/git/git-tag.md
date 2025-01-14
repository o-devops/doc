---

tags:
  - Git

---

# Les tag git

Les tags git sont une fonctionnalitée native de Git qui associe à un commit précis un tag, le tag va permettre de se repérer dans l'historique Git et de le pointer de manière facilement repérable.

Généralement les tags vont permettre de se repérer et de noter par exemple à quel moment exactement une version est sortie : dans ce cas là on utilisera un numéro de version comme tag.

Vous pouvez rapidement consulter l'historique de tags ainsi :

```bash
git tag
```

La création d'un tag est très simple il suffit de faire suivre git tag d'un numéro de version :

```bash
git tag v0.0.1
```

Vous avez ainsi créé un tag qui pointe vers votre dernier commit.

Attention un tag est créé localement et n'est pas sychronisé avec la branche distante, si vous souhaitez le publiez sur la branche distante il faut alors le spécifier directement :

```bash
git push origin v0.0.1
```

Ou plus simple pusher tous les tags non pushé :

```bash
git push origin --tags
```

## Récuperer la version du dépôt avec un tag en particulier

Avec ce point de repère vous pourrez maintenant à tout moment récupérer le contenu du dépôt à cet historique :

```bash
git checkout v0.0.1
```

!!! warning
    Attention quand vous checkout un tag il faut savoir que vous n'êtes plus reliée à une branche, vous être en mode "headless" donc vous ne pourrez pas valider vos modification sans créer de branche (ajoutez l'option `-b` au checkout)


## Tags légers ou annotés ?

Il y a deux types de tag, les tags "légers" et les tags annotés, les tags annotés ajoutent la possibilité de noter des informations contextuelles dont en particulier un message, il suffit d'ajouter l'option `-a` :

```bash
git tag -a v0.0.2 -m "Nouvelle version avec support des tags"
```

D'une manière générale il est recommandé de **toujours utiliser des tags annotés**.

## Tagguer un tag après coup

Vous pouvez tagguer un commit de votre historique après coup :

```bash
git log
git tag -a v0.0.1 97e24003948b42306acddd7068fbe8f584532f31 -m "Cette version là marchait très bien sur l'ubuntu 22.04"
```