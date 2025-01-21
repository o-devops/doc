# Les coûts AWS

Un des succès d'AWS est sa capacité à gérer finement les coûts associé à chacun des services. AWS est en capacité de mesurer très précisemsent votre consommation pour vous la facturer. Si cela peut faire peur, au final c'est tout l'inverse vous avez un bon contrôle de votre consommation et vous êtes en mesure de l'analyser.

Afin de savoir optimiser le coût de nos hébergements nous allons nous pencher sur le système de gestion des coûts d'AWS

## Les modèles de tarification

Voici quelques modèles de tarifications associés à des services AWS.

### Le compute (calcul)

Ce modèle facture le temps d'utilisation d'une ressources de calcul (compute), par ressource de calcul il faut comprendre le temps d'utilisation d'une VM.

Si vous n'utilisez une VM que 20h par mois, alors vous ne serez facturé à la fin du mois d'un coût correspond vraiment aux 20h d'utilisation, par contre le revers de la médaille c'est que vous avez la responsabilité de stopper/arrêter la VM en question : une instance EC2 démarrée est pour AWS une instance EC2 utilisée et donc facturée.

### Le stockage

L'utilisation de la ressource de stockage correspond au fait qu'elle existe pour AWS, qu'elle soit utilisée ou pas, tant qu'elle existe et qu'il faut conserver la donnée, elle est considérée comme utilisée et donc facturée.

Pour vous représenter ce coût considérez une disque dur virtuel de 10Go, si vous le provisionnez début avril, et que vous ne l'avez pas détruit au courant du mois d'avril vous serez facturé en fin de mois pour un usage de stochage total de 10Go sur une durée d'un mois.

La facturation peut varier selon les catégories de disque, mais le schéma de facturation sera toujours le même.

### Le transfert de donnée

L'utilisation de la bande passante est aussi facturée, mais selon des modalités particulières :

- le trafic sortant est facturé selon la destination (régional AWS moins cher)
- le trafic entrant n'est pas facturé
- la facturation se fait au volume

## Les modes de paiement

- Le mode par défaut est le mode à la demande, **on demand**, on paye ce que l'ont consomme au plein tarif.
- Le mode réservé **reserved**, on essaye de prédire notre consommation à l'avance pour bénéficier de tarif préferentiel.

Si vous avez des charges variable avec très peu de serveurs qui tourne en permanence, priviligiez le mode ondemand et sinon vous pouvez mettre en place des réservation de ressources, même si c'est lourd à suivre administrativement vous pouvez faire d'importante économies.

## Le calculateur de prix

Les calculs de coûts associé à une hébergement AWS sont fastidieux à calculer, ce [calculateur](https://calculator.aws/#/) vous donne la possibilité de faire quelques simulation afin de préparer votre projet.
