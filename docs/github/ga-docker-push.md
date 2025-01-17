---

tags:
  - Github Action
  - Docker

---

# Préparer et upload des images Docker avec Github Action

La préparation et la mise à disposition des images Docker est une étape clé de toute chaine de livraison moderne. Avoir des méthodes et une bonne connaissance de ces techniques vous donnera toutes les bases pour appréhender correctement le déploiement des applications sur des stack modernes d'orchestrateur de container.

## La recette de construction de vos images

Le fichier **Dockerfile** contient toutes les consignes pour construire vos images. Il faut donc stocker et versionner ce fichier dans votre dépôt de code, généralement à la racine de votre projet sous le nom `Dockerfile`.

Cette recette pourra être utilisé par plusieurs éléments de votre workflow :

- l'environnement devcontainer utilisé par le développeur pour développer et tester localement l'application,
- l'environnement de build qui produit les fichiers statiques ou les binaires associé à votre programme,
- l'environnement de production qui exécutera votre application dans la vrai situation de production,
- éventuellement dans un environnement de pré-production.

Ces différents objectifs pour une seule et même recette nécessite de mettre en place une recette qui s'appuie sur du [multi-staging Docker](https://docs.docker.com/build/building/multi-stage/)

Un bon exemple est celui de ce projet de documentation [ici](https://github.com/o-devops/doc/blob/main/Dockerfile)

## Le registre Docker

Votre image, une fois buildée, doit être mise à disposition via un registre Docker. Plusieurs solutions s'offrent à vous :

- Utiliser le **registre par défaut** : le **Dockerhub**. Situé à l'adresse docker.io ce registre est gratuit pour stocker des images publiques, payant pour stocker des images privées. La commande docker push utilisera par défaut ce registre si on ne le spécifie pas. Votre image sera dispo directement dans le [Docker hub](https://hub.docker.com)
- Utiliser le **registre de Github**. Associé à votre dépôt votre image sera directement disponible depuis l'interface de Github. De plus en utilisant le token GITHUB_TOKEN vous n'aurez pas de crédentials à fournir lors de votre build.
- Utiliser un **registre cloud alternatif** comme par exemple **ECR (AWS)** pour Amazon Web Service ou **Artifact Registry (GCP)** pour Google Cloud Platform.

## Se connecter au registre Docker

La première étape est de se connecter au registre Docker, pour cela deux techniques :

- (**à éviter**) Utiliser une action `run` et se connecter en utilisant la commande Docker `login`

```yaml
- run: docker login -u aurelienbras -p $DOCKERHUB_TOKEN
  env:
    DOCKERHUB_TOKEN: ${{ secrets.DOCKERHUB_TOKEN }}
```

Il faut bien sûr au préalable générer notre secret avec le TOKEN fournit par l'interface de docher hub.

- (**A préférer**) Utiliser l'action adapté [docker/login-action](https://github.com/marketplace/actions/docker-login)

```yaml
        name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
```

Cette méthode est à privilégier et vous permet de trouver toute les paramétrage pour chaque registre Docker.

## Builder et pusher l'image Docker

Même si vous exécuter directement les commandes Docker via deux commandes `run` build et push ainsi :

```yaml
       - run: |
           docker build -t aurelienbras/mavache:1 .
           docker push aurelienbras/mavache:1
         env:
           DOCKERHUB_TOKEN: ${{ secrets.DOCKERHUB_AURELIEN_TOKEN }}
```

Ce n'est pas pratique et il vaut mieux utilier l'action faite pour ça : [docker/build-push-action](https://github.com/marketplace/actions/build-and-push-docker-images)

```yaml
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: user/app:latest
```

Cette action combine à la fois l'étape de build et l'étape de push sur la registry.

Vous pouvez la combiner avec d'autre actions :

- [docker/metadata-action](https://github.com/marketplace/actions/docker-metadata-action) pour préparer en amont le versionning et le tag en extrayant des données relatives à l'event Github action (push, commit, tag, scheduler)

- [docker/setup-buildx-action](https://github.com/marketplace/actions/docker-setup-buildx) pour utiliser un autre builder Docker plus moderne et puissant que celui par défaut. Utile dans certains cas, même si le builder de l'ubuntu 24.04 vous suffira dans la plupart des cas.

- [docker/setup-qemu-action](https://github.com/marketplace/actions/docker-setup-qemu) cette action, utilisé au préalable de l'action précédente vous permettra de produire des image Docker dans une gamme plus large d'architecture. Inutile dans les cas ou vous déployez des architecture standard, il est très utilisé dans les projets à large distribution vous le rencontrerez souvent dans le projets publics open source.

## Exemple de workflow complet

```yaml
---

name: try docker push image

on:
  workflow_dispatch:
  push:
    tags:
      - 'v*'

env:
  REGISTRY: docker.io
  IMAGE_NAME: aurelienbras/myvache

jobs:
  build:
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@v4

      - name: Docker meta
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: |
            ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_AURELIEN_TOKEN }}
      
      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

Ici le tag Git sera utilisé pour versionner l'image Docker.

## Méthode originale pour versionner son image Docker

```yaml
      - name: Set version tag
        id: version
        run: |
          VERSION_TAG="v${{ github.run_number }}"
          echo "VERSION_TAG=$VERSION_TAG" >> $GITHUB_ENV

      - name: Build and Push Docker Image
        uses: docker/build-push-action@v6
        with:
            context: .
            push: true
            platforms: linux/amd64,linux/arm64,linux/arm/v7
            tags: |
              ${{ secrets.DOCKER_USERNAME }}/cowsay.go:latest
              ${{ secrets.DOCKER_USERNAME }}/cowsay.go:${{ env.VERSION_TAG }}
```

Récupéré d'un travail original [ici](https://github.com/no-more-name-available/cowsay-go/blob/dev/.github/workflows/deploy.yml)