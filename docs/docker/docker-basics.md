# Bases Docker

Docker est un système de container un peu particulier dans le sens qu'il se contente de faire exécuter un seul processus et de l'isoler dans un container.

Par design un container Docker ne peut donc faire tourner qu'une seule application, même si dans la pratique on trouvera souvent plusieurs processus pour faire tourner une application il faut bien veiller à respecter cette bonne pratique de ne pas essayer de mettre plusieurs applications dans un même container.

Si vous avez une application qui nécessite plusieurs composant distincts, alors votre application devra composer avec plusieurs container, par exemple en utilisant `docker compose`.

## Exécuter un container

```bash
docker run <nomDeLImage>
```

Cette commande (en fait `docker container run`) va télécharger l'image indiqué et l'exécuter via un container.

Deux options courante :

- `--rm` pour effacer le container à la fin de son exécution
- `-it` lancer le container en mode interactif (par exemple pour y exécuter un shell dedans)

### debug un container

```bash
docker run -it --rm aurelienbras/mavache /bin/bash
```

L'option `-it` combiné au passage de l'argument `/bin/bash` force à lancer la commande bash dans le container et à connecter l'entrée/sortie sur celle de notre terminal ce qui nous permet d'y taper toutes les commande qu'on y veut.

## Builder une nouvelle image

Le succès de Docker est en partie du à la grande facilité à créer ses propres images, voici ce dont vous besoin pour y arriver :

### Un fichier Dockerfile

Le fichier doit se nommé précisement ainsi `Dockerfile`

```dockerfile
FROM ubuntu:24.04

RUN apt update && apt install -y cowsay && rm -rf /var/lib/apt

ENTRYPOINT ["/usr/games/cowsay"]

CMD ["Bonjour les DevOps !"]
```

Cet exemple simple montre une image Docker simple qui installe et exécute par défaut un cowsay. Son paramètre par défaut `Bonjour les DevOps` peut être très facilement surchargé en ajoutant un argument après le nom de l'image lors du docker run.

### Builder l'image

```bash
docker build -t mavache .
```

!!! warning
    Attention le `.` de la fin est très utile et permet de définit le contexte qui sera passé au builder Docker, ici le `.` signifie qu'on lui envoie le répertoire courant et que donc potentiellement le builder peut accéder à tous les fichiers de ce répertoire. Ici il utilisera uniquement le fichier `Dockerfile`.

### Se connecter à une registry

```bash
docker login
```

Par défaut cette commande va vous connecter sur la registry par défaut : celle du dockerhub.

### Upload l'image sur une registry

```bash
docker tag mavache aurelienbras/mavache
docker push aurelienbras/mavache
```

Il faut bien penser à tagguer votre image avec un nom compatible avec le nom de votre compte sur la registry Docker.
