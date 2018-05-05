# Git

## Définitions

* Mainline : La ligne de développement de référence depuis la quelle les builds sont créés, qui sont utilisés pour le déploiement.
* Feature Branching : Une pratique ou les gens ne mergent pas leur code dans la mainline tant que la feature n'est pas terminée.
* Intégration continue : Une pratique pour s'assurer que le logiciel est toujours fonctionnel, et pour avoir des feedbacks permanent à chaque changements

## Feature Branching is evil

Source : [https://www.meetup.com/fr-FR/Software-Craftsmanship-Lille/events/250063082/?eventId=250063082&from=ref](https://www.meetup.com/fr-FR/Software-Craftsmanship-Lille/events/250063082/?eventId=250063082&from=ref)

[https://speakerdeck.com/tdpauw/agile-belgium-meetup-201802-feature-branching-considered-evil](https://speakerdeck.com/tdpauw/agile-belgium-meetup-201802-feature-branching-considered-evil)

![](/assets/31945345_10156452115494885_1204198049706934272_n.jpg)

### But d'une organisation

Minimiser le lead time pour créer un impact business positif.

### Les raisons d'avoir des feature branches qui restent longtemps

* Permet de travailler en isolation, ce qui augmente la productivité de chacun

Développer de façon isolée va permettre à un individu \(ou groupe d'individu\) d'aller plus vite, mais ça ne permet pas à l'équipe d'aller plus vite.

Le temps passé à Merge et à retravailler une feature ne peut pas être estimé, et varie beaucoup. L'équipe ne peut aller qu'aussi vite que le merge le plus lent.

* Si on s'apperçoit qu'un refactoring sur une branche ne donne rien de bon au final, on peut juste le supprimer.

Au lieu de se lancer sur des expérimentations en se basant sur le code de production, mieux vaut faire un "spike" \(poc\) afin d'explorer le potentiel d'une solution, répondre à un problème donné et ignorer le reste, puis supprimer le poc une fois que le savoir-faire a été acquis pour développer l'idée sur le code de prod.

* On peut contrôller la qualité de ce qui va en production

L'objectif est déliminer un candidat à la release le plus tôt possible dans le process.

Éliminer un candidat contenant beaucoup de contenu est contre-productif.

* On peut contrôller quelles features vont en production

Au lieu d'utiliser le contrôle de code source pour choisire les features qui vont en production, il est préférable de développer une architecture modulaire, avec la possibilité de facilement inclure ou exclure des features au runtime ou au moment du déploiement.

### Les problèmes du long time feature branching

#### Continuous Isolation

Pendant que la mainline est en CI, la feature branche reste isolée. Si on paramètre la feature branche pour builder sur la feature branche, l'unique information d'une build réussie sur la feature branche, est que dans le cadre de la feature branche, la build succeed, ça ne veut pas dire qu'on va réussir à merger. CI stand plutôt pour Continuous Isolation dans ce cas là.

#### Promiscuous Integration

Lorsque 2 feature branche ont besoins l'une de l'autre ... Et qu'elles s'échangent des feats, on ne sait plus qui a quoi ...

#### Cache le travail au reste de l'équipe

Merger fréquemment vers la Mainline = communiquer avec l'équipe

#### Contre le refactoring

Refactor sur une branche, si une autre branche a modifié des éléments refactorisés, il va être très compliqué de merger.

#### Création d'inventory

Chaques jours de developpements, toute la valeur ajoutée par le développement est sur une feature branche, c'est du potentiel non exploité, de l'argent bloqué dans le système.

#### Augmentation du risque

évidemment merger une petite quantité de code en CI est moins risqué que merger une grande quantité de code d'un coup à intégrer.

#### Crée de la compléxité

Plus il y a de feature branche, plus il devient complexe de comprendre le système.

#### Evilness

Long time feature branching est un symptome de mauvaises pratiques

Ils sont un problèmes lorsque

* le développeur ne sait pas développer en petits incréments
* le code est trop couplé, et qu'il manque de tests

### Eviter le long time Feature branching : Trunk Based Development

Toute l'équipe développe sur la mainline : le Trunk

#### Découper les gros changements en petites incrémentations

Toujours commit sur du **VERT**.

Produire du code **DÉCOUPLÉ**.

Plein de tests **RAPIDES**.

#### Cacher les nouvelles fonctionnalitées non terminées.

Si une fonctionnalité n'est pas terminée, mais qu'une partie de son implémentation est déjà dans le code source en production, c'est perfectly fine.

#### Utiliser les branches par abstraction lors de gros refactoring![](/assets/refactoring.PNG)

On souhaite changer Bleu ciel pour à la place mettre bleu foncé. Mais les Jaunes ont un bleu.

Donc on crée une interface \(rouge\)

1. 3 Jaunes ont chacuns un bleu
2. 1 jaune a un IRouge, implémenté par bleu, et 2 jaunes ont toujours un bleu
3. 2 jaunes ont un IRouge, implémenté par bleu, 1 autre jaune a un IRouge, implémenté par BleuFoncé
4. Les 3 jaunes ont un IRouge, implémenté par BleuFoncé

#### Faire des Feature Toggles pour découpler le release de feature du déploiement de code

* Releases Toggle \(un .conf, une variable à valoriser lors de la RELEASE\)
* Ops Toggles
* Experiment Toggles
* Permission Toggles

Toggles determinés à la runtime pour les 3 derniers

Attention, car feature toggles mal fait est evil aussi.

#### Le code Reviews sans feature branche

* Pair Programming : pre-commit review
* création d'une short lived branche + Pull request : pre-merge
* Direct commit : Review tous les commits sur la mainline

### Avantages du trunk based development

Plus de commits sur la mainline

==&gt; Builds plus fréquents

==&gt; Déploiements plus fréquents

==&gt; Time To Market réduit !

==&gt; Plus de retour d'expériences, plus rapidement

