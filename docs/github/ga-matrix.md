# Les matrix

Github action propose un système de matrice pour lancer un même job selon plusieurs paramètres.

Voici un exemple de matrice qui affiche pour chaque vache des cowsay avec des animaux différents :

```yaml
---

name: show matrix

on:
  push:
  workflow_dispatch:

jobs:
  demo-matrix:
    runs-on: ubuntu-24.04
    strategy:
      max-parallel: 2
      matrix:
        modele:
          - gopher
          - cower
          - docker
          - squirrel
          - koala
          - ghostbuster
          - vader
        vache:
          - marguerite
          - geraldine
          - therese
          - josette
          - augustine
        exclude:
          - modele: docker
          - modele: squirrel
          - vache: marguerite
            modele: vader
  
    steps:
      - name: output pipeline
        id: cowsay_output_id # You want id
        uses: Code-Hex/neo-cowsay-action@v1
        with:
          message: je suis ${{ matrix.vache }}
          cow: ${{ matrix.modele }} # ascii art list: https://github.com/Code-Hex/Neo-cowsay/tree/master/cows
          cowsay_to_output: 'cowsay'
      - name: print output
        run: |
          echo "${{ steps.cowsay_output_id.outputs.cowsay }}" 
```

L'exécution qui en découle va être une exécution de toutes les combinaisons, sauf les combinaisons explicitement exclues via la directive exclude.

Par défaut l'exécution va lancer tous les jobs en parralèle, selon votre situation et la disponibilité des runners il peut être utile de les limiter avec un nombre maximal d'exécution simultanné via l'option `max-parallel`. Un dépôt Github public n'aura pas de limitations contrairement à un dépôt privé.

## Cas d'usage des matrix

Les matrice sont utilisé généralement dans deux situations :

- pour préparer des binaire compilés sur toutes les architecture cible : AMD64, ARM64, etc.
- pour effectuer des tests sur plusieurs environnement de développement : node14, node16, node18, node20, etc.