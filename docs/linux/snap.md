---
tags:
  - linux
  - ubuntu
  - snap
---

# Le gestionnaire de paquet Snap

Snap est un gestionnaire de paquet pour Linux conçu et développé par Canonical l'entreprise qui est derrière la célèbre distribution Ubuntu.

Ce gestionnaire est installé par défaut sur toutes les installations Ubuntu qu'elles soient Desktop ou Serveur.

## Libérer de l'espace disque occupé par Snap

Voici quelques opérations qu'on peut effectuer pour libérer un peu d'espace disque sur son Ubuntu.

### Supprimer le cache

Le répertoire `/var/lib/snapd/cache` occupe souvent beaucoup d'espace, son contenu peut être supprimer sans se poser de questions avec la commande ci dessous :

```bash
sudo rm -rf /var/lib/snapd/cache/*
```

### Supprimer les snaps inutilisés

Premièrement regardez la liste des snap installés :

```bash
root@monserveur:~# snap list --all
Name               Version                     Rev    Tracking       Publisher    Notes
bare               1.0                         5      latest/stable  canonical✓   base
code               cd4ee3b1                    181    latest/stable  vscode✓      classic
code               fabdb6a3                    179    latest/stable  vscode✓      disabled,classic
core18             20240920                    2846   latest/stable  canonical✓   base
core20             20240911                    2434   latest/stable  canonical✓   base
core20             20240705                    2379   latest/stable  canonical✓   base,disabled
core22             20241119                    1722   latest/stable  canonical✓   base,disabled
core22             20250110                    1748   latest/stable  canonical✓   base
gnome-3-28-1804    3.28.0-19-g98f9e67.98f9e67  198    latest/stable  canonical✓   -
gtk-common-themes  0.1-81-g442e511             1535   latest/stable  canonical✓   -
insomnia           10.3.0                      302    latest/stable  getinsomnia  -
insomnia           10.2.0                      299    latest/stable  getinsomnia  disabled
lxd                4.0.9-a29c6f1               24061  4.0/stable/…   canonical✓   disabled
lxd                4.0.10-e664786              29619  4.0/stable/…   canonical✓   -
snapd              2.66.1                      23258  latest/stable  canonical✓   snapd,disabled
snapd              2.67                        23545  latest/stable  canonical✓   snapd
```

Dans cet exemple vous avez des application comme code pour vscode, insomnia ou lxd qui peuvent être supprimé. 

!!! warning
    Les autres applications comme coreXX, snapd, bare, gnome sont indispensable au fonctionnement de votre système donc attention à les conserver.

Le package `lxd` installé par défaut n'est pas exemple pas utile si vous ne savez pas ce que c'est (c'est un système de virtualisation par container très bien mais rarement utilisé par les utilisateurs).

Vous pouvez donc le supprimer et libérer environ 800Mo d'espace disque :

```bash
sudo snap remove --purge lxd
```

